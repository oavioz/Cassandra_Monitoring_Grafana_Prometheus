# On This Repo i will explain how to configure and use Grafana and Prometheus Monitoring for Apache Cassandra
Note, that This repo contains everthing needed to lauch docker containers with [Prometheus](https://prometheus.io/) and [Grafana](https:/grafana.com/) to monitor an Apache Cassandra cluster.
The new provisioning features of Grafana 5.x are used to configure the datasource and import the dashboards.

## Prerequisites
* [Docker Compose](https://docs.docker.com/compose/install/#install-compose)
* Apache Cassandra 3.x Cluster

* [JMX Prometheus Exporter setup on the Cassandra cluster nodes](https://www.robustperception.io/monitoring-cassandra-with-prometheus/)

Note: On each C* Node,  install the exporter agent

wget https://github.com/prometheus/node_exporter/releases/download/v1.0.0-rc.1/node_exporter-1.0.0-rc.1.linux-amd64.tar.gz

tar -xzvf node_exporter-1.0.0-rc.1.linux-amd64.tar.gz

cd node_exporter-1.0.0-rc.1.linux-amd64

Run the exporter on backgroud process
nohup ./node_exporter &


## Install and Build
* Clone the repo and change directories into it
```
git https://github.com/oavioz/Cassandra_Monitoring_Grafana_Prometheus.git
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
