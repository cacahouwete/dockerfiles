FROM php:8.2.6-fpm-alpine as bin

RUN apk add --no-cache \
        ca-certificates \
        icu-libs \
        freetype \
        libpng \
        libjpeg-turbo \
        libzip \
        postgresql-dev && \
    apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        wget \
        freetype-dev \
        libpng-dev \
        libjpeg-turbo-dev \
        libzip-dev \
        zlib-dev \
        libxml2-dev \
        icu-dev \
        oniguruma-dev && \
    docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql && \
    docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install mysqli zip intl mbstring pdo pdo_mysql pdo_pgsql bcmath gd soap && \
    pecl install apcu redis && \
    docker-php-ext-enable apcu opcache redis && \
    wget https://phar.phpunit.de/phploc.phar && \
    chmod +x phploc.phar && \
    mv phploc.phar /usr/local/bin/phploc && \
    wget https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar && \
    chmod +x phpcs.phar && \
    mv phpcs.phar /usr/local/bin/phpcs && \
    wget https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar && \
    chmod +x phpcbf.phar && \
    mv phpcbf.phar /usr/local/bin/phpcbf && \
    wget https://github.com/Halleck45/PhpMetrics/raw/master/build/phpmetrics.phar && \
    chmod +x phpmetrics.phar && \
    mv phpmetrics.phar /usr/local/bin/phpmetrics && \
    wget https://cs.symfony.com/download/php-cs-fixer-v3.phar && \
    chmod +x php-cs-fixer-v3.phar && \
    mv php-cs-fixer-v3.phar /usr/local/bin/php-cs-fixer && \
    echo "memory_limit = 1024M" >> /usr/local/etc/php/php.ini &&\
    apk del .build-deps

FROM composer as src

RUN composer global config --no-plugins allow-plugins.symfony/flex true && \
    composer global config --no-plugins allow-plugins.symfony/thanks true && \
    composer global require \
        psr/log:2.0.0 \
        symfony/service-contracts:^2.5 \
        symfony/flex \
        symfony/thanks \
    	phpmd/phpmd \
    	pdepend/pdepend \
        phpstan/phpstan \
        phpstan/phpstan-symfony \
        phpstan/phpstan-dibi \
        phpstan/phpstan-doctrine \
        phpstan/phpstan-phpunit \
        phpstan/phpstan-strict-rules \
        jangregor/phpstan-prophecy \
        friendsoftwig/twigcs

FROM bin
COPY --from=bin /usr/local/bin/ /usr/local/bin/
COPY --from=src /tmp /usr/local/src/
ENV PATH /usr/local/src/vendor/bin:$PATH

RUN apk add --no-cache \
        bash \
        graphviz \
        su-exec && \
    addgroup bar && \
    adduser -D -H -G bar foo

WORKDIR /project
VOLUME  /project
