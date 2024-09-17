FROM httpd:alpine

# Install OpenSSL
RUN apk add --no-cache openssl

# Enable modules and SSL
RUN sed -i \
    -e 's/^#\(Include .*httpd-ssl.conf\)/\1/' \
    -e 's/^#\(LoadModule .*mod_ssl.so\)/\1/' \
    -e 's/^#\(LoadModule .*mod_socache_shmcb.so\)/\1/' \
    -e 's/^#\(LoadModule .*mod_proxy.so\)/\1/' \
    -e 's/^#\(LoadModule .*mod_proxy_fcgi.so\)/\1/' \
    -e 's/^#\(LoadModule .*mod_rewrite.so\)/\1/' \
    /usr/local/apache2/conf/httpd.conf


# enable ssl certificate
COPY ./docker/ssl/boilerplate.pem /usr/local/apache2/conf/server.crt
COPY ./docker/ssl/boilerplate-key.pem /usr/local/apache2/conf/server.key


# Update SSL configuration
RUN sed -i \
    -e 's|^SSLCertificateFile .*|SSLCertificateFile /usr/local/apache2/conf/server.crt|' \
    -e 's|^SSLCertificateKeyFile .*|SSLCertificateKeyFile /usr/local/apache2/conf/server.key|' \
    /usr/local/apache2/conf/extra/httpd-ssl.conf


COPY ./docker/apache/httpd-vhosts.conf /usr/local/apache2/conf/extra/httpd-vhosts.conf
# Update apache conf to include the vhost config
RUN sed -i 's,#Include conf/extra/httpd-vhosts.conf,Include conf/extra/httpd-vhosts.conf,g' /usr/local/apache2/conf/httpd.conf

RUN mkdir -p /var/log/apache2 && \
    chown -R www-data:www-data /var/log/apache2

WORKDIR /var/www/html/
COPY ./working-directory/public /var/www/html/public
RUN chown -R www-data:www-data /var/www/html


EXPOSE 80
CMD ["httpd", "-D", "FOREGROUND"]