FROM theclaymethod/ansible-base
MAINTAINER Clay Kim <clayton.kim@gmail.com>

ENV SPARK_HOME /spark
ENV SPARK_LOCATION spark_location: hdfs://foundry/tmp/spark.tgz
ENV MESOS_MASTER mesos://mesos-master:5050
ENV SPARK_SERVER_HOME /spark-jobserver/runner
ENV ZK_ENDPOINT zk://mesos-master:2181
#Install the repository
RUN sudo dpkg -i repo-deb-build-0002.deb && sudo apt-get update && sudo apt-get install sbt git

RUN git clone https://github.com/spark-jobserver/spark-jobserver.git mkdir $SPARK_SERVER_HOME

RUN git clone https://github.com/theclaymethod/docker-containers /tmp/docker-playbooks

WORKDIR /tmp/docker-playbooks/spark-jobserver

EXPOSE 8090

RUN ansible-playbook playbook.yml -c local

ENTRYPOINT ["/$SPARK_SERVER_HOME/runner/server_start.sh"]