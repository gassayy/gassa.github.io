---
layout: post
title: Prometheus Basics
author: gassa
comments: true
category: DevOps
tags: [prometheus]
---

This article summrized my understanding of Promethues and PromQL. It is not mean to cover everything since [prometheus.io](https://prometheus.io/docs/introduction/overview/) provides detailed information, but aims to give a bump start to use Prometheus in system/application monitoring.


## Run your local environment 

```shell
git clone https://github.com/prometheus/prometheus.git
cd prometheus
docker-compose up -d
```

## Basic concepts: Job, Instance

An ***instance*** is an endpoint where Prometheus can scraping. A collection of instances with the similar purpose is called ***job***. Here is an example from [Prometheus documentation](https://prometheus.io/docs/concepts/jobs_instances/) that monitors a API server deployed with 4 replicas. 

- job: the api server
  * instance 1: replicas-1 
  * instance 2: replicas-1
  * instance 3: replicas-1
  * instance 4: replicas-1

The following code snippets is the prometheus configuration used by our local environment, you can find the configuration file at `<prometheus_repository>/prometheus/prometheus.yml`. The job configuration defines the name of the job, scrape internal, scrape timeout, metrics path ant metrics schema. The job instance is defined as a list of targets, which are either directly specified as `host:port` or derived from service discovery methods, more details is available [here](https://prometheus.io/docs/prometheus/latest/configuration/configuration/).


```
scrape_configs:
- job_name: prometheus
  scrape_interval: 5s
  scrape_timeout: 5s
  metrics_path: /metrics
  scheme: http
  static_configs:
  - targets:
    - localhost:9090
- job_name: node-exporter
  scrape_interval: 5s
  scrape_timeout: 5s
  metrics_path: /metrics
  scheme: http
  dns_sd_configs:
  - names:
    - tasks.node-exporter
    refresh_interval: 30s
    type: A
    port: 9100
```

## Promethes Metrics

Prometheus stores time series as ***metrics***, which present streams of timestamped values belonging to the same metrics. The x-axis are timestamps, the y-axis are the sampled values. Even though metrics can be untyped, I would recommend to explicitly privode type information since the type information provides insight about how to use the metrics with monitoring.

There are four metric types in Prometheus: Counter, Gauge, Histogram, Summary. This article focus on the first two types. A counter metric consists of a series of cumulative samples whose value can only increase. Notice that the value resets to zero on job restart. A gauge metric represents a series of numerical value that can arbitrarily go up and down. The later two types consist of multiple correlated metrics. They are tightly coupled with the build-in `histogram_quantile()` functions, which is a very handy tool when we need to monitoring service response times or latencies. We will discuss them in a seperate post later. 


### Label and Notation

Metric labels create multi-dimension metrics, any given combination of labels for the same metric name identifies a particular dimensional instantiation of that metric. Given a metric name and a set of labels, time series are frequently identified using this notation:
```
<metric_name>{label_name=<label_value>, ...}
```
For example, `http_requests_total` records the number of HTTP request made to the different Prometheus APIs (labeled as handlers). We can go to [http://localhost:9090/graph](http://localhost:9090) and submit query `http_requests_total{method="get"}`. The query result contains a list of time series that have the same metric name but different label values. 

![query results]({{ site.url }}/pics/prometheus/query_results.png)

If we want to plot a curvy that shows a total number of requests, we can use `sum()` to aggregate them, i.e., `sum(http_requests_total{method="get", handler!="prometheus"})`. In practice, we can do more interesting queries by leverage PromQL build-in functions, [the next post]({% post_url 2019-05-10-prometheus-promql %}) will introduce PromQL queries in details.



## References:
1. [Prometheus: Design and Philosophy - why it is the way it is](https://www.youtube.com/watch?v=QgJbxCWRZ1s&feature=youtu.be)
2. [Monitoring, the Prometheus Way](https://www.youtube.com/watch?v=PDxcEzu62jk&feature=youtu.be)
3. [Metric and Label Naming Convention](https://prometheus.io/docs/practices/naming/)