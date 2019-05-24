# Zeebe + Operate in Docker

This repository contains configuration files to setup an environment to
develop with [Zeebe]. It is the recommend way for developing and is maintained
by the Zeebe team itself.

The configurations manage the following Zeebe components:

- [Zeebe] is a workflow engine for micro-services orchestration.

- [Operate](https://zeebe.io/blog/2019/04/announcing-operate-visibility-and-problem-solving/) is an operations tool for monitoring and troubleshooting live workflow instances in Zeebe.

- [Simple Monitor](https://github.com/zeebe-io/zeebe-simple-monitor) is a **community maintained** monitoring tool for development purpose. This should **not** be used in production as it has a performance impact on the broker.

For more information on using Zeebe and Operate, consult the Quickstart Guide in the [Zeebe docs](https://docs.zeebe.io/getting-started/README.html).

The `docker-compose.yml` files in this repository can be used to start a single Zeebe broker; optionally with Simple Monitor, or with Operate, along with the Elasticsearch and Kibana containers that Operate needs.

## Versions

* Zeebe 0.17
* Operate 1.0.0-alpha10
* Simple Monitor 0.14.0-SNAPSHOT

# Profiles

* [`broker-only`](broker-only/docker-compose.yml) - a single node Zeebe broker
* [`cluster`](cluster/docker-compose.yml) - a three-node cluster configuration
* [`exporter-demo](exporter-demo/docker-compose.yml) - demonstrating a simple exporter. See [zeebe-exporter-demo](https://github.com/jwulf/zeebe-exporter-demo)
* [`operate`](operate/docker-compose.yml) - a single node Zeebe broker with Operate
* [`operate-simple-monitor`](operate-simple-monitor/docker-compose.yml) - a single node Zeebe broker with Operate and Simple Monitor
* [`simple-monitor`](simple-monitor/docker-compose.yml) -  a single node Zeebe broker with Simple Monitor

## Services / Ports

The containers expose the following services:

- Zeebe broker - port 26500
- Operate - web interface http://localhost:8080 (login: demo/demo)
- ElasticSearch - port https://localhost:9200
- Kibana - port https://localhost:5601
- Simple Monitor - web interface http://localhost:8082

## Prerequisites

- [Docker](https://docs.docker.com/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Recommended

To visually inspect and manage running containers and persistent volumes, you can use [Portainer](https://portainer.io).

## Start the Containers in the Foreground

Running the containers in the foreground will tail the output from each of the containers in your console, allowing you to inspect it.

Run the following command in the directory of the profile that you want to start:

```
# change to directory of the profile to start, i.e
# cd operate/
docker-compose up
```

## Stop Containers Running in the Foreground

Closing the terminal (including terminating an ssh connection) or hitting Ctrl-C will stop the containers.

To remove the stopped containers, run the following the command in the directory of the profile that you started:

```bash
docker-compose down
```

## Run the Containers in the Background (Daemon mode)

To start the containers in the background, use the `-d` flag:

```bash
docker-compose up -d
```

## Stop Containers Running in the Background

Run the following command in this directory:

```bash
docker-compose down
```

This will stop the containers and remove them, but will keep the persistent
data folders of the containers. Therefore if you recreate the containers they
will startup with your existing data volumes.

In case you want to clean also the persistent data use the following command
instead:

```bash
docker-compose down -v
```

## Removing Persistent Data

The Operate profiles create persistent volumes. Sometimes you want to flush the data from previous starts. To do this you need to delete the `zeebe_data` and `zeebe_elasticsearch_data` volumes. They are prefixed by the profile name. You can use [Portainer](https://portainer.io) to do this, or using the command line:

### List Persistent Volumes

```
docker volume ls
```

### Delete Persistent Volumes

Stop the running containers first using `docker-compose down` in the directory of the profile you started. Then:

```
# Example for the operate profile
docker volume rm operate_zeebe_data
docker volume rm operate_zeebe_elasticsearch_data
```

## Running with Simple Monitor

One thing that Operate doesn't have is inspection of messages. This can be useful when developing and debugging.

The `operate-simple-monitor` folder contains a docker-compose file that will start Operate _and_ Simple Monitor. Simple Monitor will be running on [http://localhost:8082](http://localhost:8082).


[Zeebe]: https://zeebe.io
