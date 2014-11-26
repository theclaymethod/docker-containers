# Spark Job Server

This is a Docker Container for Running Spark's Job Server on Mesos using Marathon. Designed to work with The Foundry: https://github.com/theclaymethod/Foundry-vagrant-mesos-kafka-cluster

Configure by overriding environment variables:

```
ENV SPARK_HOME /spark
ENV SPARK_LOCATION spark_location: hdfs://foundry/tmp/spark.tgz
ENV MESOS_MASTER mesos://mesos-master:5050
ENV SPARK_SERVER_HOME /spark-jobserver/runner
ENV ZK_ENDPOINT zk://mesos-master:2181
ENV SPARK_BUILD spark-1.1.0-bin-cdh4
```