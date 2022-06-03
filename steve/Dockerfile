FROM alpine:3.15

MAINTAINER Anirudh Ramesh

WORKDIR /opt/steve

ENV DOCKERIZE_VERSION v0.6.1
ENV STEVE_VERSION v22.02
ENV DATABASE_ENGINE_NAME mariadb
ENV DATABASE_SERVER_PORT 3306
ENV TZ Asia/Kolkata

COPY steve-$STEVE_VERSION.tar.gz /tmp/steve.tar.gz
COPY dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz /tmp/dockerize.tar.gz

RUN set -ex && \
    apk add --no-cache --no-progress openjdk11-jre-headless openssl tzdata

RUN cp /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN tar -C /opt/steve -xzvf /tmp/steve.tar.gz && \
    rm /tmp/steve.tar.gz

RUN tar -C /usr/local/bin -xzvf /tmp/dockerize.tar.gz \
    && rm /tmp/dockerize.tar.gz

CMD dockerize -wait tcp://$DATABASE_ENGINE_NAME:$DATABASE_SERVER_PORT -timeout 60s && \
    java -jar steve.jar
