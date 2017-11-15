FROM redis:3.2

MAINTAINER Shrimp Liu <liu.shrimp@gmail.com>

VOLUME /data

ARG PUID=1000
ARG PGID=1000

ENV PUID ${PUID}
ENV PGID ${PGID}

RUN usermod -u ${PUID} redis
RUN groupmod -g ${PGID} redis

EXPOSE 6379

CMD ["redis-server"]
