FROM phusion/baseimage:latest

MAINTAINER Paulo Dias <prbdias@gmail.com>

RUN DEBIAN_FRONTEND=noninteractive
RUN locale-gen en_US.UTF-8
COPY . .
RUN cat  /etc/apt/sources.list
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && mv sources.list /etc/apt/
RUN cat  /etc/apt/sources.list

RUN apt-get update && apt-get install -y tzdata

ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV LC_CTYPE=UTF-8
ENV LANG=en_US.UTF-8
ENV TERM xterm

# Install "software-properties-common" (for the "add-apt-repository")
RUN apt-get update && apt-get install -y \
    software-properties-common

# Add the "PHP 7" ppa
RUN add-apt-repository -y \
    ppa:ondrej/php

# Install PHP-CLI 7.3, some PHP extentions and some useful Tools with APT
RUN apt-get update && apt-get install -y --force-yes \
        php7.3-cli \
        php7.3-common \
        php7.3-curl \
        php7.3-json \
        php7.3-xml \
        php7.3-mbstring \
#        php7.3-mcrypt \
        php7.3-mysql \
        php7.3-zip \
        php7.3-gd \
        php-memcached \
        php-xdebug \
        git \
        curl \
        nano \
        iputils-ping \
        zip \
        unzip


# Clean up, to free some space
RUN apt-get clean

# Add an alias for PHPUnit
RUN echo "alias phpunit='./vendor/bin/phpunit'" >> ~/.bashrc

# Install Composer
RUN curl -s http://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/ \
    && echo "alias composer='/usr/local/bin/composer.phar'" >> ~/.bashrc

# Source the bash
RUN . ~/.bashrc

#RUN echo "Asia/Shanghai" > /etc/timezone
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN dpkg-reconfigure -f noninteractive tzdata

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV COMPOSER_ALLOW_SUPERUSER 1

ADD init.sh /sbin/init.sh
RUN chmod +x /sbin/init.sh
CMD ["/sbin/init.sh"]

WORKDIR /var/www
