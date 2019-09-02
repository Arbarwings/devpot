FROM php:7.3-fpm

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils

# Install locale
ARG TZ=UTC
ENV TZ ${TZ}
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
    && apt-get install -f -y --reinstall locales && locale-gen en_US.UTF-8

# INSTALL EXTENSIONS

# apcu
RUN pecl install apcu
RUN echo "extension=apcu.so" > /usr/local/etc/php/conf.d/apcu.ini

# bz2
RUN apt-get install -y libbz2-dev
RUN docker-php-ext-install bz2

# gd
RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd

# mcrypt
RUN apt-get install -y libmcrypt-dev
RUN pecl install mcrypt-1.0.2
RUN docker-php-ext-enable mcrypt

RUN apt-get install -y openssl libssl-dev

# pdo
RUN docker-php-ext-install pdo_mysql  \
    && apt-get install -y libpq-dev \
    && docker-php-ext-install pdo_pgsql \
    && apt-get install -y libsqlite3-dev \
    && docker-php-ext-install pdo_sqlite \
    && docker-php-ext-install mysqli

# phpredis
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

# xsl
RUN apt-get install -y libxslt-dev
RUN docker-php-ext-install xsl

# zip
RUN apt-get install -y libzip-dev \
    && docker-php-ext-install zip

# intl
RUN apt-get update && apt-get install -y zlib1g-dev libicu-dev g++
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl

# common
RUN docker-php-ext-install opcache calendar dba pcntl bcmath mbstring xmlrpc ftp shmop

# preconf enviroment
ENV PHP_EXTRA_CONFIGURE_ARGS --enable-fpm --with-fpm-user=www-data --with-fpm-group=www-data

RUN usermod -u 1000 -d /data www-data -s /bin/bash
RUN groupadd -r node && \
    useradd -r -g node node && \
    usermod -G www-data -a node

RUN mkdir /data && chmod -R 644 /data && find /data -type d -exec chmod 755 {} \;
RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        msmtp \
        imagemagick \
        libssl-dev \
        libxml2-dev \
        libicu-dev \
        libxslt-dev \
        wget git vim libcurl4-openssl-dev \
        mc

# Install OH-MY-ZSH to see pretty terminal and ditch the bash
ENV ZSH=/home/.oh-my-zsh
RUN apt-get install -y zsh
RUN curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | bash

# Clean apt
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#COMPOSER
ENV COMPOSER_HOME=/home/composer/.composer
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN curl https://getcomposer.org/installer | php -- && mv composer.phar /usr/local/bin/composer && chmod +x /usr/local/bin/composer
ENV PATH /home/composer/.composer/vendor/bin:$PATH

#PHPUNIT
RUN composer global require "phpunit/phpunit" && \
    ln -s /home/composer/.composer/vendor/bin/phpunit /usr/local/bin/phpunit

#Laravel
RUN composer global require "laravel/installer"
RUN ln -s /home/composer/.composer/vendor/bin/laravel /usr/local/bin/laravel

COPY etc/php-fpm.conf /usr/local/etc/
COPY etc/php.ini /usr/local/etc/php/
COPY etc/.bashrc /etc/bash.bashrc
COPY etc/.zshrc /etc/zsh/newuser.zshrc.recommended
COPY etc/.zshrc /etc/zsh/zshrc
RUN chmod ugo+rX -R /usr/local/etc/php
RUN chmod -R 777 /home/composer && find /home/composer -type d -exec chmod 777 {} \;

WORKDIR /data

CMD ["php-fpm"]
