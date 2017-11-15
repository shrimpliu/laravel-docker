FROM phusion/baseimage:latest

MAINTAINER Shrimp Liu <liu.shrimp@gmail.com>

## 安装supervisor
RUN apt-get update \
  && apt-get install -y supervisor \
  && rm -rf /var/lib/apt/lists/* \
  && sed -i 's/^\(\[supervisord\]\)$/\1\nnodaemon=true/' /etc/supervisor/supervisord.conf

VOLUME /etc/supervisor/conf.d

## 安装php及对应扩展
RUN apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:ondrej/php

RUN apt-get update \
  && apt-get install -y --allow-downgrades --allow-remove-essential \
    --allow-change-held-packages \
    php7.1-cli \
    php7.1-common \
    php7.1-curl \
    php7.1-json \
    php7.1-xml \
    php7.1-mbstring \
    php7.1-mcrypt \
    php7.1-mysql \
    php7.1-zip \
    php7.1-bcmath \
    php7.1-memcached \
    php7.1-gd \
    php7.1-dev

RUN pecl install -o -f redis \
  && rm -rf /tmp/pear \
  && echo "extension=redis.so" > /etc/php/7.1/cli/conf.d/20-redis.ini  

## 清理
RUN apt-get clean

WORKDIR /etc/supervisor/conf.d

CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf"]