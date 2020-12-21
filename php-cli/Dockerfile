ARG PHP_VERSION=7.4

FROM php:${PHP_VERSION}-cli-alpine

RUN apk add --no-cache bash
RUN apk add --no-cache libsodium
RUN apk add --no-cache libsodium-dev
RUN apk add --no-cache libxslt-dev
RUN apk add --no-cache icu-libs
RUN apk add --no-cache icu-dev
RUN apk add --no-cache libxslt-dev
RUN apk add --no-cache git
RUN apk add --no-cache libzip-dev
RUN apk add --no-cache zip
RUN apk add --no-cache unzip
RUN apk add --no-cache postgresql-dev
RUN apk add --no-cache $PHPIZE_DEPS

RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install sodium
RUN docker-php-ext-install xsl
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl
RUN docker-php-ext-install zip
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install pdo_pgsql
RUN docker-php-ext-install exif
RUN docker-php-ext-install soap
RUN docker-php-ext-install bcmath

RUN pecl install xdebug

RUN cp $(find /usr/local/etc/php -name "php.ini-development") /usr/local/etc/php/php.ini

RUN find /usr/local/lib/php -name "xdebug.so" -exec echo 'zend_extension="{}"' >> /usr/local/etc/php/php.ini \;

COPY xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Install composer & make composer global
RUN curl -s https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

RUN mkdir /.composer && chown 1000:1000 /.composer

USER 1000:1000

WORKDIR /var/htdocs
