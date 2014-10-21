FROM debian:wheezy
MAINTAINER Tom Hill <tom@greensheep.io>

# Install packages
RUN apt-get update \
 && apt-get upgrade -y
RUN apt-get install -y \
     autoconf \
     build-essential \
     imagemagick \
     libgd2-xpm-dev \
     mcrypt \
     libmcrypt-dev \
     libbz2-dev \
     libcurl4-openssl-dev \
     libevent-dev \
     libffi-dev \
     libglib2.0-dev \
     libjpeg-dev \
     libmagickcore-dev \
     libmagickwand-dev \
     libmysqlclient-dev \
     libncurses-dev \
     libpq-dev \
     libreadline-dev \
     libsqlite3-dev \
     libssl-dev \
     libxml2-dev \
     libxslt-dev \
     libyaml-dev \
     zlib1g-dev

# Build and install PHP
ADD ./lib /home/lib
WORKDIR /home/lib/php-5.5
RUN tar -xvf php-5.5.18.tar.gz
WORKDIR /home/lib/php-5.5/php-5.5.18
RUN ./configure \
      --enable-fpm \
      --enable-zip \
      --with-curl \
      --with-freetype-dir \
      --with-gd \
      --with-imagick \
      --with-jpeg-dir \
      --with-mcrypt \
      --with-mysqli \
      --with-mysql \
      --with-openssl \
      --with-pdo-mysql \
      —-with-zlib
RUN make clean
RUN make
RUN make install
RUN cp sapi/fpm/php-fpm /usr/local/bin

# PHP config files
ADD ./conf/php/php.ini /usr/local/php/php.ini
ADD ./conf/php/php-fpm.conf /usr/local/etc/php-fpm.conf
