# Prometheus training sessions

This repository contains step-by-step training sessions for learning Prometheus.

Requirements: you need to have ```docker``` and ```docker-compose``` installed (e.g. by installing Docker Desktop https://www.docker.com/products/docker-desktop) and are able to access https://hub.docker.com/

## Diagrams

### 01_First_steps

![01_First_steps](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/images/01.svg?sanitize=true)

This first step configures a Prometheus instance, that monitorins itself and a Grafana instance, which is connected to Prometheus.

See https://prometheus.io/docs/prometheus/latest/configuration/configuration for Prometheus basic configuration options.

### 02_Adding_node-exporter

![02_Adding_node-exporter](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/images-exporter/02.svg?sanitize=true)

In this step we add node-exporter (https://github.com/prometheus/node_exporter), which is the de-facto standard to pull in system-level metrics of Linux and Unix based machines.
For Windows machines the wmi-exporter can be used (https://github.com/martinlindhe/wmi_exporter).

In this step we also change Prometheus configuration to use file-based service dicovery (https://prometheus.io/docs/prometheus/latest/configuration/configuration/#file_sd_config) using the directory ```config/prometheus/targets/```, which makes it possible to add or remove scrape targets on the fly, without the need to restart Prometheus.

### 03_Adding_an_instrumented_application

![03_Adding_an_instrumented_application](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/images/03.svg?sanitize=true)

In this step we add Minio (https://min.io/) - an open-source S3-compatible storage - as an example of applications, which are already instrumented to deliver Prometheus metrics. Applications of this type offer an HTTP endpoint from which metrics can be scraped.

### 04_Adding_applications_through_exporters

![04_Adding_applications_through_exporters](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/images/04.svg?sanitize=true)

Now we add Redis (https://redis.io/) to the mix. Redis does not offer Prometehus metrics by itself, it needs an exporter as an itermediate piece, in this case https://github.com/oliver006/redis_exporter . This exporter "knows" how to query Redis for useful metrics, turns them into a Prometheus-comaptible format and provides and HTTP endpoint to scrape from.

### 05_Alerting

![05_Alerting](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/images/05.svg?sanitize=true)

We now add alerting to our setup. Prometheus itself evaluates "alert rules" (https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/). If an alert is "firing" it forwards it to the Alertmanager (https://prometheus.io/docs/alerting/alertmanager/). The Alertmanager can route, group, deduplicate or silence alerts to different targets, e.g. mail, Slack or Pagerduty.

There is a pseudo-mail-server configured in this session to see how genereated mail alerts look like.

An alert rule for Redis is configured in ```confog/prometheus/redis.rules```. When we stop the Redis-server using ```docker-compose -p training stop redis-server``` we will see an alert firing in Prometheus, being forwarded to the Alertmanager and then snet out by mail.

### 06_Adding_Thanos

![06_Adding_Thanos](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/images/06.svg?sanitize=true)

Our next step is to add Thanos (https://github.com/improbable-eng/thanos) to the session. Thanos adds easy long-term-storage - e.g. to Amazon S3 - to Prometheus, as well as HA capabilities and possibilites to group multiple Prometheus instances into a fleet-wide overview. See https://github.com/m-kraus/prometheus_experiments/tree/master/thanos for a more detailed setup.

### 07_DNS_service_discovery

![07_DNS_service_discovery](https://raw.githubusercontent.com/m-kraus/prometheus_training/master/images/07.svg?sanitize=true)

In the last step we will get to know how (DNS-)service discovary works. We took node-exporter out of the file_sd targets in ```config/prometheus/targets``` and added a ```dns_sd_configs``` section in the Prometheus configuration file ```config/prometheus/prometheus.yml```

We search for DNS A records with the value ```node-exporter```. After starting the session, Prometheus discovers one node-exporter. After scaling it to multiple replicas with ```docker-compose -p training scale node-exporter=3``` Prometheus will discover after a short time all 3 instances and scrape them separately.

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
