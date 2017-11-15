FROM mysql:5.7

MAINTAINER Shrimp Liu <liu.shrimp@gmail.com>

ARG PUID=1000
ARG PGID=1000

ENV PUID ${PUID}
ENV PGID ${PGID}

RUN usermod -u ${PUID} mysql
RUN groupmod -g ${PGID} mysql

RUN chown -R mysql:root /var/lib/mysql/

ADD my.cnf /etc/mysql/conf.d/my.cnf

CMD ["mysqld"]

EXPOSE 3306
