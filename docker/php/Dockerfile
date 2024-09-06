FROM php:8.2-fpm-alpine

# Install dependencies
RUN apk update && apk add --no-cache \
    linux-headers \
    build-base \
    libzip-dev \
    zip \
    unzip \
    git \
    curl \
    oniguruma-dev
# Required for the GD extension, which handles image creation and manipulation.
# Remove if not using image processing.
RUN apk add --no-cache freetype-dev libjpeg-turbo-dev libpng-dev


# Install PHP extensions
RUN docker-php-ext-install pdo_mysql opcache mbstring zip exif pcntl
# Remove these also if not using image processing.
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd

# Install Redis extension
RUN apk add --no-cache pcre-dev $PHPIZE_DEPS \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && apk del pcre-dev $PHPIZE_DEPS

# Install Xdebug
RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && apk del $PHPIZE_DEPS


# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer


# Copy the codebase in the working dir of the container
WORKDIR /var/www/html
COPY ./applications /var/www/html
RUN chown -R www-data:www-data /var/www/html
USER www-data


# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm", "-y", "/usr/local/etc/php-fpm.conf", "-R"]