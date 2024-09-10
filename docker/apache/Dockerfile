FROM httpd:alpine

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