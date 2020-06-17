# On this Repo i will explain how to configure and use Grafana and Prometheus Monitoring for Apache Cassandra
Note, that This repo contains everthing needed to lauch docker containers with [Prometheus](https://prometheus.io/) and [Grafana](https:/grafana.com/) to monitor an Apache Cassandra cluster.
The new provisioning features of Grafana 5.x are used to configure the datasource and import the dashboards.

## Prerequisites
* [Docker Compose](https://docs.docker.com/compose/install/#install-compose)
* Apache Cassandra 3.x Cluster

* [JMX Prometheus Exporter setup on the Cassandra cluster nodes](https://www.robustperception.io/monitoring-cassandra-with-prometheus/)
* I have h jmx exsample file you can use it. located on "./prometheus/cassnadra-jmx.yml"
* Please do the follow:
* Step 1. Download JMX-Exporter:
$ mkdir /opt/jmx_prometheus
  $ wget https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.3.0/jmx_prometheus_javaagent-0.3.0.jar

* Step 2. Configure JMX-Exporter
  $ vim /opt/jmx_prometheus/cassnadra-jmx.yml

* Step 3. Configure Cassandra
  echo 'JVM_OPTS="$JVM_OPTS -javaagent:/opt/prometheus-exporter/jmx_prometheus_javaagent-0.3.0.jar=7070:/<your exporter installation>/prometheus-exporter/cassnadra-jmx.yml"' &amp;gt;&amp;gt; conf/cassandra-env.sh
* Step 4. Restart Cassandra
 $ nodetool flush
   $ nodetool drain
   $ sudo service cassandra restart


* On each C* Node,  install the exporter agent as follow:

1. wget https://github.com/prometheus/node_exporter/releases/download/v1.0.0-rc.1/node_exporter-1.0.0-rc.1.linux-amd64.tar.gz

2. tar -xzvf node_exporter-1.0.0-rc.1.linux-amd64.tar.gz

3. cd node_exporter-1.0.0-rc.1.linux-amd64

Run the exporter on backgroud process
4. nohup ./node_exporter &


## Install and Build
* Clone the repo and change directories into it
```
git clone https://github.com/oavioz/Cassandra_Monitoring_Grafana_Prometheus.git
cd cassandra-monitoring

```
* Edit the json files `prometheus/tg_cassandra.json` and `prometheus/tg_node.json` and define your collection endpoints
```
[
  {
    "targets": [ 
      "xxx.xx.xx.xxx:7070", 
      "xxx.xx.xx.xxx:7070", 
      "xxx.xx.xx.xxx:7070"
    ],
    "labels": {
      "cluster": "Demo_cluster"
    }
  }
]
```
```
[
  {
    "targets": [ 
      "xxx.xx.xxx.xx:9100", 
      "xxx.xx.xxx.xx:9100", 
      "xxx.xx.xxx.xx:9100"
    ],
    "labels": {
      "cluster": "Demo_cluster"
    }
  }
]
```

docker-compose up -d
```
That's it! If there were no erros you can open a browser and visit the Grafana interface and login (http://localhost:3000/)
