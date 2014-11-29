FROM sequenceiq/pam:ubuntu-14.04
MAINTAINER Clay Kim <clayton.kim@gmail.com>

RUN apt-get -y update && apt-get install -y python-yaml python-jinja2 git
RUN git clone http://github.com/ansible/ansible.git --recursive /tmp/ansible
WORKDIR /tmp/ansible
RUN hacking/env-setup
ENV PATH /tmp/ansible/bin:/sbin:/usr/sbin:/usr/bin
ENV ANSIBLE_LIBRARY /tmp/ansible/library
ENV PYTHONPATH /tmp/ansible/lib:$PYTHON_PATH


ENV SPARK_HOME /spark
ENV SPARK_LOCATION hdfs://foundry/tmp/spark.tgz
ENV MESOS_MASTER zk://mesos-master.foundry.project.com:2181/mesos
ENV SPARK_SERVER_HOME /spark-jobserver/runner
ENV ZK_ENDPOINT zk://mesos-master:2181
ENV SPARK_BUILD spark-1.1.0-bin-cdh4
ENV MESOS_HOME /usr/local/lib/libmesos.so
ENV DEB_VERSION_MESOS 0.21.0-1.0.debian77

#Install the repository
ENV PATH /tmp/ansible/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:$PATH

WORKDIR /
RUN echo "deb http://repos.mesosphere.io/debian wheezy main" > /etc/apt/sources.list.d/mesosphere.list && \
	apt-key adv --keyserver keyserver.ubuntu.com --recv E56151BF && \
	apt-get install -y curl && \
	curl -O http://apt.typesafe.com/repo-deb-build-0002.deb && \
	dpkg -i repo-deb-build-0002.deb && \
	apt-get update && \
	apt-get install -y --no-install-recommends sbt \
		git-core \
		build-essential \
		apt-utils \
		mesos=$DEB_VERSION_MESOS && \
	rm repo-deb-build-0002.deb 

WORKDIR /tmp
RUN curl -O http://d3kbcqa49mib13.cloudfront.net/$SPARK_BUILD.tgz && \
	tar -xvf $SPARK_BUILD.tgz && \
	mv $SPARK_BUILD /spark && \
	rm $SPARK_BUILD.tgz


RUN git clone https://github.com/theclaymethod/docker-containers /tmp/docker-playbooks
# Install SBT
# RUN mkdir -p //.sbt/.lib/0.13.5
# WORKDIR //.sbt/.lib/0.13.5
# RUN curl -O http://typesafe.artifactoryonline.com/typesafe/ivy-releases/org.scala-sbt/sbt-launch/0.13.5/sbt-launch.jar

ADD sbt/ /tmp/sbt/
RUN chmod +x /tmp/sbt/sbt.sh

#clone jobserver repo
RUN git clone https://github.com/spark-jobserver/spark-jobserver.git /tmp/spark-jobserver && \ 
	cd /tmp/spark-jobserver && \
	mv /tmp/sbt /tmp/spark-jobserver/sbt && \
	sbt/sbt.sh job-server/assembly && \
	mkdir -p $SPARK_SERVER_HOME && \
	cp /tmp/spark-jobserver/bin/server_start.sh $SPARK_SERVER_HOME && \
	cp /tmp/spark-jobserver/bin/server_stop.sh $SPARK_SERVER_HOME && \
	cp /tmp/spark-jobserver/job-server/target/spark-job-server.jar $SPARK_SERVER_HOME && \
	rm -rf /tmp/spark-jobserver


EXPOSE 8090 8091 7077 7078 4040


WORKDIR /tmp
#clone ansible playbook

WORKDIR /tmp/docker-playbooks/spark-jobserver
CMD ansible-playbook -i inventory playbook.yml
