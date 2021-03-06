# Flink docker

Apache Flink docker images to:
* Setup a standalone [Apache Flink](http://flink.apache.org/) cluster running one Flink Master and multiple Flink workers
* Build Flink applications in Scala, Java or Python to run on a Flink cluster

Currently supported versions:
* Flink 1.2.1 for Hadoop 2.7 and Scala 2.11
* Flink 1.2.0 for Hadoop 2.7 and Scala 2.11
* Flink 1.1.4 for Hadoop 2.7 and Scala 2.11
* Flink 1.1.3 for Hadoop 2.7 and Scala 2.11
* Flink 0.10.2 for Hadoop 2.7 and Scala 2.11
* Flink 0.10.1 for Hadoop 2.7 and Scala 2.11

## Using Docker Compose

Add the following services to your `docker-compose.yml` to integrate a Flink master and Flink worker in [your BDE pipeline](https://github.com/big-data-europe/app-bde-pipeline):
```
flink-master:
   image: bde2020/flink-master
   hostname: flink-master
   container_name: flink-master
   environment:
      - INIT_DAEMON_STEP=setup_flink
#     - "constraint:node==<yourmasternode>"

   ports:
     - "8080:8080"
     - "8081:8081"

 flink-worker:
   image: bde2020/flink-worker
   hostname: flink-worker
   container_name: flink-worker
   environment:
     - FLINK_MASTER_PORT_6123_TCP_ADDR=flink-master
#    - FLINK_NUM_TASK_SLOTS=2
#    - "constraint:node==<yourworkernode>"
   depends_on:
      - "flink-master"

```

## Running Docker containers without the init daemon
## Flink Master
To start a Flink master:

    docker run --name flink-master -h flink-master -e ENABLE_INIT_DAEMON=false -d bde2020/flink-master:latest

## Flink Worker
To start a Flink worker:

    docker run --name flink-worker --link flink-master:flink-master -e ENABLE_INIT_DAEMON=false -d bde2020/flink-worker:latest

## Launch a Flink application
Building and running your Flink application on top of the Flink cluster is as simple as extending a template Docker image. Check the template's README for further documentation.
* [Maven template](https://github.com/big-data-europe/docker-flink/tree/master/template/maven)
* [Sbt template](https://github.com/big-data-europe/docker-flink/tree/master/template/sbt)
