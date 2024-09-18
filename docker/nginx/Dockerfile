FROM nginx:stable-alpine

COPY ./docker/nginx/templates /etc/nginx/templates
COPY ./docker/nginx/conf.d/*.conf /etc/nginx/conf.d/

# enable ssl certificate
RUN mkdir -p /etc/ssl
COPY ./docker/ssl /etc/ssl

COPY ./working-directory/public /var/www/html/public
WORKDIR /var/www/html

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]