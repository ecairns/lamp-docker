FROM ubuntu:latest
MAINTAINER Ethan Cairns <ecairns@oklahoman.com>

# Install base packages
RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install -y \
      apache2 \
      mcrypt php7.0-mcrypt \
      pkg-config \
      libfreetype6-dev \
      libjpeg62-dev \
      libmcrypt-dev \
      libpng12-dev \
      php7.0 \
      php7.0-dev \
      php7.0-cli \
      php7.0-soap \
      php7.0-curl \
      libapache2-mod-php7.0 \
      php7.0-gd \
      php7.0-json \
      php7.0-ldap \
      php7.0-mbstring \
      php7.0-mysql \
      php7.0-pgsql \
      php7.0-sqlite3 \
      php7.0-xml \
      php7.0-xsl \
      php-pear \
      php7.0-zip 

#####################################
# PHP REDIS EXTENSION FOR PHP 7.0
#####################################
ARG INSTALL_PHPREDIS=false
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear 

COPY ./redis.ini /etc/php/7.0/apache2/conf.d/10-redis.ini
COPY ./redis.ini /etc/php/7.0/cli/conf.d/10-redis.ini

#####################################
# PHP Memcached:
#####################################
ARG INSTALL_MEMCACHED=true
RUN apt-get update && apt-get install -y \
    curl \
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
    && rm /tmp/memcached.tar.gz 

COPY ./memcached.ini /etc/php/7.0/apache2/conf.d/10-memcached.ini
COPY ./memcached.ini /etc/php/7.0/cli/conf.d/10-memcached.ini


#####################################
# Opcache:
#####################################
COPY ./opcache.ini /etc/php/7.0/apache2/conf.d/10-opcache.ini

#####################################

COPY apache_default /etc/apache2/sites-available/000-default.conf
COPY run /usr/local/bin/run

COPY ./php.ini /etc/php/7.0/apache2/php.ini
COPY ./php.ini /etc/php/7.0/cli/php.ini

RUN cp /etc/apache2/mods-available/vhost_alias.* /etc/apache2/mods-enabled
RUN chmod +x /usr/local/bin/run
RUN a2enmod rewrite


RUN usermod -u 1000 www-data
WORKDIR /var/www

EXPOSE 80
CMD ["/usr/local/bin/run"]