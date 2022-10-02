FROM php:8.1.11-fpm-alpine as bin

RUN apk add --no-cache \
        ca-certificates \
        icu-libs \
        git \
        unzip \
        postgresql-dev \
        freetype \
        libpng \
        libjpeg-turbo \
        freetype-dev \
        libpng-dev \
        libjpeg-turbo-dev \
        libzip \
        libzip-dev \
        zlib-dev \
        libxml2-dev \
        oniguruma-dev

RUN apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        icu-dev

RUN docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql
RUN docker-php-ext-configure gd \
    --with-freetype \
    --with-jpeg
RUN docker-php-ext-install mysqli zip intl mbstring pdo pdo_mysql pdo_pgsql bcmath gd soap

RUN pecl install apcu redis
RUN docker-php-ext-enable apcu opcache redis

RUN apk add --no-cache \
        wget && \
    wget https://phar.phpunit.de/phploc.phar && \
    chmod +x phploc.phar && \
    mv phploc.phar /usr/local/bin/phploc && \
    wget https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar && \
    chmod +x phpcs.phar && \
    mv phpcs.phar /usr/local/bin/phpcs && \
    wget https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar && \
    chmod +x phpcbf.phar && \
    mv phpcbf.phar /usr/local/bin/phpcbf && \
    wget https://phar.phpunit.de/phpcpd.phar && \
    chmod +x phpcpd.phar && \
    mv phpcpd.phar /usr/local/bin/phpcpd && \
    wget https://phar.phpunit.de/phpdcd.phar && \
    chmod +x phpdcd.phar && \
    mv phpdcd.phar /usr/local/bin/phpdcd && \
    wget https://github.com/Halleck45/PhpMetrics/raw/master/build/phpmetrics.phar && \
    chmod +x phpmetrics.phar && \
    mv phpmetrics.phar /usr/local/bin/phpmetrics && \
    wget https://cs.symfony.com/download/php-cs-fixer-v3.phar && \
    chmod +x php-cs-fixer-v3.phar && \
    mv php-cs-fixer-v3.phar /usr/local/bin/php-cs-fixer && \
    echo "memory_limit = 1024M" >> /usr/local/etc/php/php.ini

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
