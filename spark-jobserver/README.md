# Spark Job Server

This is a Docker Container for Running Spark's Job Server on Mesos using Marathon. Designed to work with The Foundry: https://github.com/theclaymethod/Foundry-vagrant-mesos-kafka-cluster

Configure by overriding environment variables:

```
{
    "id": "/spark-jobserver",
    "instances": 1,
    "cpus": 0.25,
    "mem": 512,
    "env": {
        "MESOS_MASTER": "zk://100.0.10.11:2181/mesos"
    },
    "container": {
        "type": "DOCKER",
        "docker": {
            "image": "theclaymethod/spark-jobserver-mesos",
            "network": "HOST"
        }
    },
    "ports" : [8090]
}
```