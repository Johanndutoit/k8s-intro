FROM ubuntu:latest
MAINTAINER "Johann du Toit" <me@johanndutoit.net>

# add the packages
RUN apt-get update
RUN apt-get install wget curl -y

# run the item
RUN mkdir -p /app

# add the items
ONBUILD ADD . /app

# Default work dir
WORKDIR /app

# install supervisor to be able to run nginx and fpm
RUN apt-get install -y supervisor

# install essentials
RUN apt-get install -y build-essential

# install php
RUN apt-get install -y php7.0 \ 
  php7.0-curl \
  php7.0-mbstring \
  php7.0-mcrypt \
  php7.0-mysql \
  php7.0-xml \
  php7.0-fpm

# install nginx
RUN apt-get install -y nginx

# copy nginx files
COPY ./image/nginx.conf /etc/nginx/nginx.conf
COPY ./image/nginx.app.conf /etc/nginx/conf.d/app.conf

# copy over fpm settings
RUN rm /etc/nginx/sites-enabled/default
RUN rm /etc/php/7.0/fpm/pool.d/www.conf
COPY ./image/php.ini /etc/php/7.0/fpm/php.ini
COPY ./image/fpm.conf /etc/php/7.0/fpm/php-fpm.conf
COPY ./image/www1.conf /etc/php/7.0/fpm/pool.d/www1.conf
COPY ./image/www2.conf /etc/php/7.0/fpm/pool.d/www2.conf

# setup 
COPY ./image/supervisord.conf /etc/supervisord.conf

# add the items
ADD . /app

# set /app/ permissions
RUN chmod -R g+rwx /app/
RUN chown -R www-data:www-data /app/

# start and run all our services
CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisord.conf"]