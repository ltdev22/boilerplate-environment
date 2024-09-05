FROM nginx:stable-alpine

COPY ./docker/nginx/conf.d/app.conf /etc/nginx/conf.d/default.conf
COPY ./docker/nginx/conf.d/*.conf /etc/nginx/conf.d/

COPY ./applications/public /var/www/html/public
WORKDIR /var/www/html

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]