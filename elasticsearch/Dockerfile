FROM elasticsearch:5.4.0

MAINTAINER Shrimp Liu <liu.shrimp@gmail.com>

RUN apt-get update && apt-get install zip

RUN mkdir -p /usr/share/elasticsearch/plugins/ik
ADD elasticsearch-analysis-ik-5.4.0.zip /usr/share/elasticsearch/plugins/ik/
RUN cd /usr/share/elasticsearch/plugins/ik && \
    unzip elasticsearch-analysis-ik-5.4.0.zip && \
    rm elasticsearch-analysis-ik-5.4.0.zip

# 自定义词库
ADD mydict.dic /usr/share/elasticsearch/plugins/ik/config/custom/

ARG PUID=1000
ARG PGID=1000

ENV PUID ${PUID}
ENV PGID ${PGID}

RUN usermod -u ${PUID} elasticsearch
RUN groupmod -g ${PGID} elasticsearch

EXPOSE 9200 9300
