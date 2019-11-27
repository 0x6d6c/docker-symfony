FROM php:7.3.11-cli-stretch

RUN curl -s https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update && apt-get install -yqq \
        libicu-dev \
        libzip-dev \
        zip \
        nodejs \
        yarn \
        git && \
        docker-php-ext-configure zip --with-libzip && \
        docker-php-ext-install intl && \
        docker-php-ext-install zip && \
        docker-php-ext-install pdo_mysql

ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN set -xe; \
    groupadd -g ${PGID} developer && \
    useradd -u ${PUID} -g developer -m developer && \
    usermod -p "*" developer -s /bin/bash && \
    cp $(find /usr/local/etc/php -name "php.ini-development") /usr/local/etc/php/php.ini

COPY .bash_aliases /home/developer/.bash_aliases

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

USER developer

WORKDIR /var/htdocs
