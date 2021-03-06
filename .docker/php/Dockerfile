#-------------------------------------#
#   version PHP (default value)(latest version unsupported)
ARG PHP_VERSION=7.1.33
#-------------------------------------#

FROM php:${PHP_VERSION}-fpm
MAINTAINER Gilles BOUCHONNEAU <gilles.bouchonneau@gmail.com>

#-------------------------------------#
#   Install and update packages
#-------------------------------------#
RUN apt-get update \
    && apt-get install -y --no-install-recommends vim curl debconf subversion \
    git apt-transport-https apt-utils \
    build-essential locales acl mailutils wget nodejs \
    libzip-dev \
    zip \zip unzip \
    gnupg gnupg1 gnupg2 \
    zlib1g-dev \
    sudo ssh-client

#-------------------------------------#
#   extensions PHP
#-------------------------------------#
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions gd pdo pdo_mysql zip amqp pdo_pgsql pdo_firebird pdo_sqlsrv opcache memcached xdebug apcu


COPY php.ini /usr/local/etc/php/php.ini

#-------------------------------------#
#   Install phpunit
#-------------------------------------#
RUN wget --no-check-certificate https://phar.phpunit.de/phpunit-7.phar \
    && mv phpunit*.phar phpunit.phar \
    && chmod +x phpunit.phar \
    && mv phpunit.phar /usr/local/bin/phpunit \
    ./phpunit --version


#-------------------------------------#
#   Install locales
#-------------------------------------#
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
    && echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen \
    && echo "dev ALL=(ALL) ALL" > /etc/sudoers
#-------------------------------------#
#  CLEANING
#-------------------------------------#
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/*

#-------------------------------------#
#   COMPOSER
#-------------------------------------#
RUN curl -sSk https://getcomposer.org/installer | php -- --disable-tls && \
	mv composer.phar /usr/local/bin/composer

#-------------------------------------#
#   NODE
#-------------------------------------#
RUN	echo "deb https://deb.nodesource.com/node_8.x jessie main" >> /etc/apt/sources.list.d/nodejs.list && \
	wget -nv -O -  https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add - && \
	echo "deb-src https://deb.nodesource.com/node_8.x jessie main" >> /etc/apt/sources.list.d/nodejs.list && \
    apt-get update && \
	apt-get install -y --force-yes nodejs && \
	rm -f /etc/apt/sources.list.d/nodejs.list

#-------------------------------------#
#   YARN
#-------------------------------------#
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
	echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install yarn


#-------------------------------------#
#   Permissions and more !
#-------------------------------------#
WORKDIR /var/www/html
RUN usermod -u 1000 www-data
EXPOSE 9000
CMD ["php-fpm"]

#-------------------------------------#
# FIN
#-------------------------------------#
