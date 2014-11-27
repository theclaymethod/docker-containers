FROM theclaymethod/ansible-base
MAINTAINER Clay Kim <clayton.kim@gmail.com>

ENV SPARK_HOME /spark
ENV SPARK_LOCATION hdfs://foundry/tmp/spark.tgz
ENV MESOS_MASTER mesos://mesos-master:5050
ENV SPARK_SERVER_HOME /spark-jobserver/runner
ENV ZK_ENDPOINT zk://mesos-master:2181
ENV SPARK_BUILD spark-1.1.0-bin-cdh4
ENV MESOS_HOME /usr/local/lib/libmesos.so
#Install the repository
ENV PATH /tmp/ansible/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:$PATH

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y git-core build-essential apt-utils curl
RUN curl -O http://apt.typesafe.com/repo-deb-build-0002.deb && \
 dpkg -i repo-deb-build-0002.deb && \
 apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y sbt

#RUN mkdir $SPARK_SERVER_HOME

#Install SBT
RUN mkdir -p /root/.sbt/.lib/0.13.5
WORKDIR /root/.sbt/.lib/0.13.5
RUN curl -O http://typesafe.artifactoryonline.com/typesafe/ivy-releases/org.scala-sbt/sbt-launch/0.13.5/sbt-launch.jar

#clone ansible playbook
RUN git clone https://github.com/theclaymethod/docker-containers /tmp/docker-playbooks

#clone jobserver repo
RUN git clone https://github.com/spark-jobserver/spark-jobserver.git /tmp/spark-jobserver

WORKDIR /tmp/spark-jobserver
RUN sbt job-server/assembly

WORKDIR /tmp/docker-playbooks/spark-jobserver

RUN ansible-playbook -i inventory playbook.yml

RUN cp /tmp/spark-jobserver/bin/server_start.sh $SPARK_SERVER_HOME && \
	cp /tmp/spark-jobserver/bin/server_stop.sh $SPARK_SERVER_HOME && \
	cp /tmp/spark-jobserver/job-server/target/spark-job-server.jar $SPARK_SERVER_HOME


ENV DEB_VERSION_MESOS 0.21.0-1.0.debian77

RUN echo "deb http://repos.mesosphere.io/debian wheezy main" > /etc/apt/sources.list.d/mesosphere.list
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv E56151BF
RUN apt-get update

RUN apt-get -f -y install mesos=$DEB_VERSION_MESOS

EXPOSE 8090

WORKDIR /spark-jobserver/runner/
CMD ./server_start.sh