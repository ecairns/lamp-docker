FROM php:fpm

MAINTAINER Ethan Cairns <ecairns@oklahoman.com>

#
# Installing iconv, mcrypt and gd extensions,
RUN apt-get update && apt-get install -y \
	libfreetype6-dev \
	libjpeg62-turbo-dev \
	libmcrypt-dev \
	libpng12-dev \
	&& docker-php-ext-install -j$(nproc) iconv mcrypt \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd

#####################################
# pdo_mysql:
#####################################
RUN docker-php-ext-install pdo pdo_mysql


#####################################
# xDebug:
#####################################
ARG INSTALL_XDEBUG=false
RUN pecl install xdebug && docker-php-ext-enable xdebug 
# Copy xdebug configration for remote debugging
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

#####################################
# PHP REDIS EXTENSION FOR PHP 7.0
#####################################
ARG INSTALL_PHPREDIS=false
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis 


#####################################
# PHP Memcached:
#####################################
ARG INSTALL_MEMCACHED=true
RUN apt-get update && apt-get install -y \
    libmemcached-dev && \
	curl -L -o /tmp/memcached.tar.gz "https://github.com/php-memcached-dev/php-memcached/archive/php7.tar.gz" \
    && mkdir -p memcached \
    && tar -C memcached -zxvf /tmp/memcached.tar.gz --strip 1 \
    && ( \
        cd memcached \
        && phpize \
        && ./configure \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r memcached \
    && rm /tmp/memcached.tar.gz \
    && docker-php-ext-enable memcached 

#####################################
# Opcache:
#####################################
RUN \
    docker-php-ext-install opcache && \
    docker-php-ext-enable opcache 

# Copy opcache configration
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

#
#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------
#

COPY php.ini /usr/local/etc/php/
ADD ./laravel.ini /usr/local/etc/php/conf.d
ADD ./laravel.pool.conf /usr/local/etc/php-fpm.d/

RUN rm -r /var/lib/apt/lists/*

RUN usermod -u 1000 www-data

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
