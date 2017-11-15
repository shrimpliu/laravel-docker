FROM laradock/workspace:1.8-71

MAINTAINER Shrimp Liu <liu.shrimp@gmail.com>

## 添加非root用户
ARG PUID=1000
ARG PGID=1000

ENV PUID ${PUID}
ENV PGID ${PGID}

RUN groupadd -g ${PGID} shrimp && \
    useradd -u ${PUID} -g shrimp -m shrimp && \
    apt-get update -yqq

## 设置时区

RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone

## 添加composer

COPY ./composer.json /home/shrimp/.composer/composer.json

RUN chown -R shrimp:shrimp /home/shrimp/.composer

## 添加ssh

ADD insecure_id_rsa /tmp/id_rsa
ADD insecure_id_rsa.pub /tmp/id_rsa.pub

USER root
ENV SSH_DIR /root/.ssh
RUN rm -f /etc/service/sshd/down && \
    cat /tmp/id_rsa.pub >> $SSH_DIR/authorized_keys && \
    cat /tmp/id_rsa.pub >> $SSH_DIR/id_rsa.pub && \
    cat /tmp/id_rsa >> $SSH_DIR/id_rsa && \
    rm -f /tmp/id_rsa* && \
    chmod 644 $SSH_DIR/authorized_keys $SSH_DIR/id_rsa.pub && \
    chmod 400 $SSH_DIR/id_rsa

## 安装node, npm

USER shrimp
ENV NODE_VERSION 6.9.4
ENV NVM_DIR /home/shrimp/.nvm
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash && \
        . $NVM_DIR/nvm.sh && \
        nvm install ${NODE_VERSION} && \
        nvm use ${NODE_VERSION} && \
        nvm alias ${NODE_VERSION}

RUN echo "" >> ~/.bashrc && \
    echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc        

## 安装redis扩展
USER root
RUN pecl install -o -f redis \
  && rm -rf /tmp/pear \
  && echo "extension=redis.so" > /etc/php/7.1/cli/conf.d/20-redis.ini

## sql server
RUN cd / \
  && apt-get update \
  && apt-get install -y --force-yes wget apt-transport-https curl freetds-common libsybdb5 freetds-bin unixodbc unixodbc-dev \
  && echo "deb http://packages.dotdeb.org jessie all" | tee /etc/apt/sources.list.d/dotdeb.list \
  && wget -qO- https://www.dotdeb.org/dotdeb.gpg | apt-key add - \
  && apt-get update -y \
  && apt-get upgrade -y \
  && apt-get install -y whiptail \ 
  unixodbc libgss3 odbcinst devscripts debhelper dh-exec dh-autoreconf libreadline-dev libltdl-dev \
  && dget -u -x http://cdn-fastly.deb.debian.org/debian/pool/main/u/unixodbc/unixodbc_2.3.1-3.dsc \
  && cd unixodbc-*/ \
  && ./configure && make && make install \
  && cp -v ./exe/odbc_config /usr/local/bin/ \ 
  && printf '#!/bin/bash\nif [ "$*" == "-p" ]; then echo "x86_64"; else /bin/uname "$@"; fi' \
  | tee /usr/local/bin/uname \
  && chmod +x /usr/local/bin/uname \
  && cd / \
  && wget -nv -O msodbcsql-13.0.0.0.tar.gz \
  "https://meetsstorenew.blob.core.windows.net/contianerhd/Ubuntu%2013.0%20Tar/msodbcsql-13.0.0.0.tar.gz?st=2016-10-18T17%3A29%3A00Z&se=2022-10-19T17%3A29%3A00Z&sp=rl&sv=2015-04-05&sr=b&sig=cDwPfrouVeIQf0vi%2BnKt%2BzX8Z8caIYvRCmicDL5oknY%3D" \
  && tar -xf msodbcsql-13.0.0.0.tar.gz \
  && cd msodbcsql-*/ \
  && ldd lib64/libmsodbcsql-13.0.so.0.0 \
  && ./install.sh install --accept-license \
  && ls -l /opt/microsoft/msodbcsql/ \
  && odbcinst -q -d -n "ODBC Driver 13 for SQL Server" \
  && pecl install sqlsrv-4.0.8 \ 
  && pecl install pdo_sqlsrv-4.0.8 \ 
  && apt-get install -y locales \
  && echo "zh_CN.UTF-8 UTF-8" > /etc/locale.gen \
  && locale-gen \
  && echo "extension=sqlsrv.so" > /etc/php/7.1/cli/conf.d/20-sqlsrv.ini \
  && echo "extension=pdo_sqlsrv.so" > /etc/php/7.1/cli/conf.d/20-pdo_sqlsrv.ini


## 添加crontab
USER root
COPY ./crontab /etc/cron.d
RUN chmod -R 644 /etc/cron.d

## 清理
USER root
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

## default work directory
WORKDIR /var/www
