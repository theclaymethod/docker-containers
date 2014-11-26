FROM theclaymethod/ansible-base
MAINTAINER Clay Kim <clayton.kim@gmail.com>

ENV SPARK_HOME /spark
ENV SPARK_LOCATION spark_location: hdfs://foundry/tmp/spark.tgz
ENV MESOS_MASTER mesos://mesos-master:5050
ENV SPARK_SERVER_HOME /spark-jobserver/runner
ENV ZK_ENDPOINT zk://mesos-master:2181
ENV SPARK_BUILD spark-1.1.0-bin-cdh4
#Install the repository
ENV PATH /tmp/ansible/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:$PATH

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y git-core build-essential apt-utils curl
RUN curl -O http://apt.typesafe.com/repo-deb-build-0002.deb && \
 dpkg -i repo-deb-build-0002.deb && \
 apt-get update && \
 DEBIAN_FRONTEND=noninteractive apt-get install -y sbt

RUN git clone https://github.com/spark-jobserver/spark-jobserver.git

#RUN mkdir $SPARK_SERVER_HOME

#Install SBT
RUN mkdir -p /root/.sbt/.lib/0.13.5
WORKDIR /root/.sbt/.lib/0.13.5
RUN curl -O http://typesafe.artifactoryonline.com/typesafe/ivy-releases/org.scala-sbt/sbt-launch/0.13.5/sbt-launch.jar

RUN rm -r /tmp/docker-playbooks
WORKDIR /tmp
RUN git clone https://github.com/theclaymethod/docker-containers /tmp/docker-playbooks

WORKDIR /tmp/docker-playbooks/spark-jobserver

# RUN pwd
# RUN ls ../

RUN ansible-playbook -i inventory playbook.yml

EXPOSE 8090

ENTRYPOINT ["spark-jobserver/runner/server_start.sh"]