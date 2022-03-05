ARG DOCKER_PHP_VERSION

FROM php:${DOCKER_PHP_VERSION}-fpm as base

RUN set -eux; \
    apt update && \
    apt install -y \
    libfreetype6-dev \
    libmcrypt-dev \
    libwebp-dev \
    libjpeg-dev \
    libpng-dev \
    libicu-dev \
    libzip-dev && \
    rm -rf /var/lib/apt/lists/*

# < 7.4
#RUN docker-php-ext-configure gd \
#    --with-freetype-dir=/usr \
#    --with-jpeg-dir=/usr \
#    --with-png-dir=/usr \
#    --with-webp-dir=/usr

RUN docker-php-ext-configure gd --with-jpeg --with-webp --with-freetype

RUN docker-php-ext-install \
    gd \
    intl \
    zip \
    pdo_mysql

RUN set -eux; \
    docker-php-ext-enable opcache

FROM base as php-fpm-dev

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
COPY xDebug.ini /usr/local/etc/php/conf.d/xdebug.ini

RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini
RUN sed -i '/cgi.fix_pathinfo=1/s/^;//g' /usr/local/etc/php/php.ini


FROM base as php-fpm-prod

RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
RUN sed -i '/cgi.fix_pathinfo=1/s/^;//g' /usr/local/etc/php/php.ini