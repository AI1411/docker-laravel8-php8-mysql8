FROM php:fpm-alpine

RUN apk update && \
    apk add --update --no-cache \
     shadow \
     bash && \
    docker-php-ext-install pdo_mysql

RUN apk add autoconf build-base && \
    pecl install xdebug && \
    docker-php-ext-enable xdebug

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apk add --update nodejs nodejs-npm

ARG PUID=1000
ARG PGID=1000
ARG USER_NAME="user"

ENV PUID ${PUID}
ENV PGID ${PGID}

RUN groupadd -g ${PGID} ${USER_NAME}
RUN useradd -u ${PUID} -g ${USER_NAME} -m ${USER_NAME}
RUN usermod -p "*" ${USER_NAME} -s /bin/sh

USER ${USER_NAME}
RUN composer global require "laravel/installer" && composer global require "phpunit/phpunit"
ENV PATH $PATH:/home/${USER_NAME}/.composer/vendor/bin

WORKDIR /var/www/app
