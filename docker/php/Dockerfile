FROM php:7.2.8-fpm-alpine

USER root

RUN apk add --no-cache \
        git \
        icu-dev \
        libmemcached-libs \
        zlib-dev \
    && apk add --no-cache --virtual .ext-build-deps \
        libmemcached-dev \
                    
    && pecl install -o \
        memcached-3.1.3 \
    && docker-php-ext-enable \
        memcached \
    && docker-php-ext-install -j5 \
        intl \
        opcache \
        zip \
	&& pecl install xdebug \
    && docker-php-ext-enable xdebug \
	&& apk del .xdebug-build-deps \
    && pecl clear-cache \
                    
	&& docker-php-source delete \
    && apk del .ext-build-deps

COPY xdebug/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

WORKDIR /var/www/my-project

RUN adduser -D -u 1000 aurelien -G www-data \
    && mkdir -p /home/aurelien \
    && chown -R aurelien /home/aurelien \
    && chown -R aurelien /var/www/my-project \
    && curl -sS https://getcomposer.org/installer | php -- \
        --install-dir=/usr/local/bin --filename=composer \
        --version 1.8.0

USER aurelien

ARG $GITHUB_TOKEN
                
RUN composer config -g github-oauth.github.com $GITHUB_TOKEN \
    && composer global require \
        --no-suggest --no-interaction \
        hirak/prestissimo \
    && rm -rf ~/.composer/cache
