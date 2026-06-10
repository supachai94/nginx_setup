FROM php:8.1-fpm-alpine

RUN apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        mariadb-dev \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install opcache \
    && apk del .build-deps

# Copy PHP-FPM pool configuration
COPY www.conf /usr/local/etc/php-fpm.d/www.conf

# Copy PHP configuration
COPY php.ini /usr/local/etc/php/php.ini
