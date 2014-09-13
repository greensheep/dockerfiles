FROM ubuntu:14.04
MAINTAINER Tom Hill <tom@greensheep.io>

# Add libraries directory
ADD ./lib /home/lib

##########################
## INSTALL DEPENDENCIES ##
##########################

# Install packages
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get upgrade -y
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential libmysqlclient-dev libxml2-dev libgd-dev libcurl4-openssl-dev

# Build and install PHP
WORKDIR /home/lib/php-5.5
RUN tar -xvf php-5.5.16.tar.gz
WORKDIR /home/lib/php-5.5/php-5.5.16
RUN ./configure --enable-fpm --with-mysql --with-gd --with-curl
RUN make
RUN make install
RUN cp sapi/fpm/php-fpm /usr/local/bin

###############
## CONFIGURE ##
###############

# PHP config files
ADD ./conf/php/php.ini /usr/local/php/php.ini
ADD ./conf/php/php-fpm.conf /usr/local/etc/php-fpm.conf
