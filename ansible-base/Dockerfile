# Ansible
#
# VERSION 0.1

FROM debian:wheezy
MAINTAINER Clay Kim <clayton.kim@gmail.com>

RUN apt-get -y update
RUN apt-get install -y python-yaml python-jinja2 git
RUN git clone http://github.com/ansible/ansible.git --recursive /tmp/ansible
WORKDIR /tmp/ansible
RUN hacking/env-setup
ENV PATH /tmp/ansible/bin:/sbin:/usr/sbin:/usr/bin
ENV ANSIBLE_LIBRARY /tmp/ansible/library
ENV PYTHONPATH /tmp/ansible/lib:$PYTHON_PATH
