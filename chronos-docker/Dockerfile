# Chronos
#
# VERSION 0.1

FROM ubuntu:trusty
MAINTAINER Clay Kim <clayton.kim@gmail.com@gmail.com>

ENV VERSION_CHRONOS 2.2.0-0.1


RUN echo "deb http://repos.mesosphere.io/ubuntu trusty main" > /etc/apt/sources.list.d/mesosphere.list
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv E56151BF
RUN apt-get update

RUN apt-get -f -y install chronos=$VERSION_CHRONOS

# we don't want to use localhost as zk_host
RUN rm /etc/mesos/zk
# don't set http_port 
RUN rm /etc/chronos/conf/http_port

EXPOSE 8080

ENTRYPOINT ["/usr/local/bin/chronos"]

