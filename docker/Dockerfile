####################################################################
#                   Dockerfile for Laravel/Lumen                   #
####################################################################

# ------------- Setup Environment -------------------------------------------------------

# Pull base image
FROM ubuntu:22.04

# Fix timezone
ENV TZ=America/Sao_Paulo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Get PHP Version
ARG PHP_VERSION

# Install common tools 
RUN apt-get update
RUN apt-get install -y \
    wget \
    curl \
    nano \
    htop \
    git \
    unzip \
    bzip2 \
    software-properties-common \ 
    locales

# Set evn var to enable xterm terminal
ENV TERM=xterm

# Set working directory
WORKDIR /var/www/html

# Add repositories
RUN LC_ALL=C.UTF-8 apt-add-repository ppa:ondrej/php
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABF5BD827BD9BF62
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4F4EA0AAE5267A6C
RUN echo "deb http://nginx.org/packages/ubuntu/ trusty nginx" >> /etc/apt/sources.list
RUN echo "deb-src http://nginx.org/packages/ubuntu/ trusty nginx" >> \ 
    /etc/apt/sources.list


# ------------- Application Specific Stuff ----------------------------------------------

# Install PHP
RUN apt-get update
RUN apt-get install -y \
    php${PHP_VERSION}-fpm \ 
    php${PHP_VERSION}-common \ 
    php${PHP_VERSION}-curl \ 
    php${PHP_VERSION}-mysql \ 
    php${PHP_VERSION}-mbstring \ 
    php${PHP_VERSION}-xml \
    php${PHP_VERSION}-bcmath \
    php${PHP_VERSION}-gd \
    php${PHP_VERSION}-xdebug
	
# config xdebug to allow run clover_coverage 
RUN echo "xdebug.mode=coverage" >> /etc/php/${PHP_VERSION}/mods-available/xdebug.ini 

# ------------- FPM & Nginx configuration -----------------------------------------------

# Config fpm to use TCP instead of unix socket
ADD data/www.conf /etc/php/${PHP_VERSION}/fpm/pool.d/www.conf
RUN mkdir -p /var/run/php

# Add user to nginx
RUN useradd -s /bin/false nginx

# Install nginx
RUN apt-get install -y nginx

# Move especific application config
ADD data/application.conf /etc/nginx/sites-enabled/

# Move nginx config
ADD data/nginx.conf /etc/nginx/

# ------------- Composer ----------------------------------------------------------------

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin \
    --filename=composer

# ------------- Supervisor Process Manager ----------------------------------------------

# Install supervisor
RUN apt-get install -y supervisor
RUN mkdir -p /var/log/supervisor
ADD data/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Add php-fpm version to supervisor config
RUN echo "[program:php-fpm]" >> /etc/supervisor/conf.d/supervisord.conf
RUN echo "command=/usr/sbin/php-fpm${PHP_VERSION} -F" >> /etc/supervisor/conf.d/supervisord.conf

# ------------- Container Config --------------------------------------------------------

# Copy application folder to docker (use only in production)
#ADD ./ /var/www/html

# Remove folder with docker configs (can't add this folder in .dockerignore because 
# it will be ignored before starts the build and not be able to ADD config file above)
#RUN rm -rf docker 

# Expose port 8000
EXPOSE 8000

# Set supervisor to manage container processes
ENTRYPOINT ["/usr/bin/supervisord"]
