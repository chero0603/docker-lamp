FROM php:7.3-fpm

RUN apt-get update

RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
				libxml2-dev \
        mailutils \
        memcached \
        msmtp \
        msmtp-mta \
        openssl \
				vim \
        apache2 \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd exif

RUN docker-php-ext-install \
        mbstring \
        json \
        simplexml \
        dom \
        fileinfo \
        hash \
        xml \
        pdo_mysql \
        mysqli

COPY ./msmtprc /etc/msmtprc
RUN chown www-data:www-data /etc/msmtprc
RUN chmod 600 /etc/msmtprc

RUN  a2enmod rewrite

COPY ./php.ini /usr/local/etc/php

COPY ./xdebug.ini $PHP_INI_DIR/conf.d/

ARG DOCKER_UID=1000
ARG DOCKER_USER=docker
ARG DOCKER_PASSWORD=docker
RUN useradd -m --uid ${DOCKER_UID} --groups sudo ${DOCKER_USER} \
  && echo ${DOCKER_USER}:${DOCKER_PASSWORD} | chpasswd

USER ${DOCKER_USER}