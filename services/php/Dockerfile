FROM php:7.3.4-fpm-alpine

USER root

# Set -e : exit immediately if a command exits with a non-zero status
# Set -x : print commands and their arguments as they are executed
RUN set -ex; \
    apk add --no-cache \
        autoconf \
        g++ \
        make \
        git \
        icu-dev \
        libmemcached-libs \
        zlib-dev \
        libzip-dev \
    # --no-cache skips the cache locally
    # --virtual creates virtual packages with dependencies
    # .build-deps is the name of the package you are creating
    && apk add --no-cache --virtual .build-deps \
        libmemcached-dev \
    && pecl install -o \
        memcached-3.1.3 \
    && docker-php-ext-enable \
        memcached \
    && docker-php-ext-install -j5 \
        intl opcache zip \
    #  install all required dependencies
    && pecl install -o xdebug \
    && docker-php-ext-enable xdebug \
    && pecl clear-cache \
    && docker-php-source delete \
    && apk del .build-deps

COPY services/php/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

WORKDIR /var/www/my-project

RUN set -ex; \
    adduser -D -u 1000 aurelien -G www-data \
    && mkdir -p /home/aurelien \
    && chown -R aurelien /home/aurelien \
    && chown -R aurelien /var/www/my-project \
    && curl -sS https://getcomposer.org/installer | php -- \
        --install-dir=/usr/local/bin --filename=composer \
        --version 1.8.5

USER aurelien

# Defined in .env file
ARG GITHUB_TOKEN

# Set -u : treat unset variables as an error when substituting
RUN set -eux; \
    composer config -g github-oauth.github.com ${GITHUB_TOKEN} \
    && composer global require \
        --no-suggest --no-interaction \
        hirak/prestissimo \
    && rm -rf ~/.composer/cache
