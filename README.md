# Prometheus training sessions

This repository contains 6 step-by-step training sessions for learning Prometheus.

Requirements: you need to have ```docker``` and ```docker-compose``` installed (e.g. by installing Docker Desktop https://www.docker.com/products/docker-desktop) and are able to access https://hub.docker.com/

## Diagrams

![01_First_steps](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/01_First_steps/01.svg?sanitize=true)

![02_Adding_node-exporter](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/02_Adding_node-exporter/02.svg?sanitize=true)

![03_Adding_an_instrumented_application](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/03_Adding_an_instrumented_application/03.svg?sanitize=true)

![04_Adding_applications_through_exporters](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/04_Adding_applications_through_exporters/04.svg?sanitize=true)

![05_Alerting](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/05_Alerting/05.svg?sanitize=true)

![06_Adding_Thanos](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/06_Adding_Thanos/06.svg?sanitize=true)

## Accessing the components

You can click on the components in the diagrams above.

For minio use the credentials ```key:secretkey```. For Grafana use the credentials ```admin:pass```.

## Useful commands

### Starting a training session

```
cd YOUR_DESIRED_LESSON
docker-compose -p training up
```

### Stopping a training session

```
docker-compose -p training down
```

### Stopping the Redis server

```
docker-compose -p training stop redis-server
```

### Sending SIGHUP to a container

```
docker kill --signal=HUP <container_id>
```

### Cleaning up after the last session

```
docker-compose -p training down -v
```