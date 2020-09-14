FROM php:7.4-fpm

RUN apt-get update

RUN apt-get install -y \
        unzip \
        git \
        vim \
        default-mysql-client \
        apache2 \
        mailutils \
        memcached \
        msmtp \
        msmtp-mta \
        openssl

RUN apt-get install -y \
        libonig-dev \
    && docker-php-ext-install iconv mbstring \
    && docker-php-ext-configure mbstring --disable-mbregex

RUN apt-get install -y \
        libcurl4-openssl-dev \
    && docker-php-ext-install curl

RUN apt-get install -y \
        libssl-dev \
    && docker-php-ext-install ftp phar

RUN apt-get install -y \
        libicu-dev \
    && docker-php-ext-install intl

RUN apt-get install -y \
        libmcrypt-dev \
    && docker-php-ext-install session

RUN apt-get install -y \
        libxml2-dev \
    && docker-php-ext-install simplexml xml xmlrpc

RUN apt-get install -y \
        libzip-dev \
        zlib1g-dev \
    && docker-php-ext-configure zip

RUN apt-get install -y \
        libgmp-dev \
    && docker-php-ext-install gmp

RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libwebp-dev \
        libxpm-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

RUN apt-get install -y libmagickwand-dev
RUN pecl install imagick && docker-php-ext-enable imagick

RUN docker-php-ext-install \
        opcache \
        calendar \
        bcmath \
        gettext \
        pdo \
        pdo_mysql \
        json \
        dom \
        fileinfo \
        mysqli

RUN a2enmod rewrite

RUN a2enmod ssl
RUN a2ensite default-ssl

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENV COMPOSER_BINARY=/usr/local/bin/composer \
    COMPOSER_HOME=/usr/local/composer
ENV PATH $PATH:$COMPOSER_HOME

RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar $COMPOSER_BINARY && \
    chmod +x $COMPOSER_BINARY

ARG DOCKER_UID=1000

RUN usermod -o -u ${DOCKER_UID} www-data \
    && groupmod -o -g ${DOCKER_UID} www-data

COPY ./php.ini /usr/local/etc/php

COPY ./msmtprc /etc/msmtprc
RUN chown www-data:www-data /etc/msmtprc
RUN chmod 600 /etc/msmtprc

COPY ./xdebug.ini $PHP_INI_DIR/conf.d/

USER www-data