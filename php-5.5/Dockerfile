FROM debian:wheezy
MAINTAINER Tom Hill <tom@greensheep.io>

# Install packages
RUN apt-get update \
 && apt-get upgrade -y
RUN apt-get install -y \
     autoconf \
     automake \
     libtool \
     re2c \
     flex \
     bison \
     build-essential \
     wget \
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
     libwebp-dev \
     libxml2-dev \
     libxslt-dev \
     libyaml-dev \
     zlib1g-dev \
 && rm -rf /var/lib/apt/lists/*

# Build and install PHP / PHP-FPM
ADD ./lib /home/lib
WORKDIR /home/lib/php-5.5
RUN tar -xvf php-5.5.18.tar.gz
WORKDIR /home/lib/php-5.5/php-5.5.18
RUN ./configure \
      --enable-fpm \
      --with-config-file-path=/etc/php5 \
      --with-mysql \
      --with-mysqli \
      --with-zlib \
      --with-jpeg-dir \
      --with-gd \
      --with-freetype-dir \
      --with-curl \
      --with-openssl \
      --with-pdo-mysql \
      --with-mcrypt \
      --enable-zip
RUN make clean
RUN make
RUN make install
RUN cp sapi/fpm/php-fpm /usr/local/bin

# PHP config files
ADD ./conf/php/php.ini /etc/php5/php.ini
ADD ./conf/php/php-fpm.conf /usr/local/etc/php-fpm.conf

# Install php-fpm init script
RUN cp sapi/fpm/init.d.php-fpm.in /etc/init.d/php-fpm \
 && chmod 755 /etc/init.d/php-fpm \
 && sed -i "s/php_fpm_BIN=@sbindir@/php_fpm_BIN=\/usr\/local\/bin/g" /etc/init.d/php-fpm \
 && sed -i "s/php_fpm_CONF=@sysconfdir@/php_fpm_CONF=\/usr\/local\/etc/g" /etc/init.d/php-fpm \
 && sed -i "s/php_fpm_PID=@localstatedir@/php_fpm_PID=\/var/g" /etc/init.d/php-fpm

# ImageMagick
RUN pear config-set php_ini /etc/php5/php.ini \
 && pecl config-set php_ini /etc/php5/php.ini
RUN printf "\n" | pecl install -f imagick

# Clean up
WORKDIR /home
RUN rm -rf ./*
