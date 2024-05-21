FROM php:8.3-fpm-alpine as app

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

RUN set -eux; \
    install-php-extensions pdo pdo_mysql;

# RUN docker-php-ext-install pdo pdo_mysql

# super user for composer
ENV COMPOSER_ALLOW_SUPERUSER=1

# composer image into php container
COPY --from=composer:2.4 /usr/bin/composer /usr/bin/composer

# order matters, frist only copy composer json/lock and run composer install, see https://medium.com/@softius/faster-docker-builds-with-composer-install-b4d2b15d0fff
COPY ./app/composer.* ./

RUN composer install --prefer-dist --no-dev --no-progress --no-interaction

COPY ./app .

RUN composer dump-autoload --optimize

# target build build this for dev
FROM app as app_dev

ENV XDEBUG_MODE=off

COPY ./php/conf.d/xdebug.ini $PHP_INI_DIR/conf.d/xdebug.ini

RUN set -eux; \
    install-php-extensions xdebug








