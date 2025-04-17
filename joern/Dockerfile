FROM alpine:latest

# 设置国内镜像源
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

# 如果需要代理，可以取消注释以下行
# RUN export http_proxy=127.0.0.1:8090
# RUN export https_proxy=127.0.0.1:8090
# RUN export no_proxy="localhost,127.0.0.1,10.96.0.0/12,192.168.0.1/16,192.168.49.2"

# dependencies
RUN apk update && apk upgrade && apk add --no-cache openjdk17-jdk python3 git curl gnupg bash nss ncurses php
RUN ln -sf python3 /usr/bin/python

# sbt
ENV SBT_VERSION 1.10.3
ENV SBT_HOME /usr/local/sbt
ENV PATH ${PATH}:${SBT_HOME}/bin
# RUN curl -sL "https://github.com/sbt/sbt/releases/download/v$SBT_VERSION/sbt-$SBT_VERSION.tgz" | gunzip | tar -x -C /usr/local
# 复制本地SBT压缩包到容器中
COPY sbt-${SBT_VERSION}.tgz /tmp/
# 解压并安装SBT
RUN mkdir -p ${SBT_HOME} && \
    tar -xzf /tmp/sbt-${SBT_VERSION}.tgz -C /usr/local && \
    rm /tmp/sbt-${SBT_VERSION}.tgz

# building joern
#RUN git clone https://github.com/joernio/joern && cd joern && sbt stage
COPY joern-master.zip /tmp/
RUN unzip /tmp/joern-master.zip -d /tmp/ && \
    mv /tmp/joern-master /joern && \
    cd /joern && \
    sbt stage

WORKDIR /joern

