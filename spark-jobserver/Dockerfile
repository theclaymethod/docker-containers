FROM theclaymethod/ansible-base
MAINTAINER Clay Kim <clayton.kim@gmail.com>

ENV SPARK_HOME /spark
ENV SPARK_LOCATION spark_location: hdfs://foundry/tmp/spark.tgz
ENV MESOS_MASTER mesos://mesos-master:5050
ENV SPARK_SERVER_HOME /spark-jobserver/runner
ENV ZK_ENDPOINT zk://mesos-master:2181
#Install the repository
ENV PATH /tmp/ansible/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y git-core build-essential apt-utils curl
RUN curl -O http://apt.typesafe.com/repo-deb-build-0002.deb && \
 dpkg -i repo-deb-build-0002.deb && \
 apt-get update && \
 DEBIAN_FRONTEND=noninteractive apt-get install -y sbt

RUN git clone https://github.com/spark-jobserver/spark-jobserver.git $SPARK_SERVER_HOME

RUN mkdir $SPARK_SERVER_HOME/runner

RUN git clone https://github.com/theclaymethod/docker-containers /tmp/docker-playbooks

WORKDIR /tmp/docker-playbooks/spark-jobserver

EXPOSE 8090

RUN ansible-playbook playbook.yml -c local -i inventory

ENTRYPOINT ["/$SPARK_SERVER_HOME/runner/server_start.sh"]