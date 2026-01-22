FROM php:8.4-fpm-alpine

RUN apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        mariadb-dev \
    && docker-php-ext-install pdo_mysql \
    && apk del .build-deps

# Copy PHP-FPM pool configuration
COPY www.conf /usr/local/etc/php-fpm.d/www.conf