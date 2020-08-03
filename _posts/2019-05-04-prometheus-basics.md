---
layout: post
title: Prometheus Basics
author: gassa
comments: true
category: DevOps
tags: [prometheus]
---

This article summarized Prometheus basic concepts for a quick guidance. More details is available at [https://prometheus.io/docs/](https://prometheus.io/docs/).


## Run your local environment 

Prometheus code base has a local bundle for demonstration purpose. The following commands will download public docker images and running them on docker.

```shell
git clone https://github.com/prometheus/prometheus.git
cd prometheus
docker-compose up -d

# http://localhost:9090            - Prometheus UI
# http://localhost:9100/metrics    - Node Export metrics
# http://localhost:8080/containers - Container Advisor
# http://localhost:9093            - Alert Manager
# http://localhost:3000            - Grafana
```

## Job and Instance

Prometheus scrape an endpoints to collects sampling points from metrics and stores them as ***time series***. The targeted endpoint is called ***instance***, it is usually corresponding to a service / a process. A collection of instances (targets) with the same purpose, e.g., a web service replicated for scalability or reliability, is called ***job***.  

For example, a web service with 4 replicas. To consume metrics from the web service, we need to configure a job with a list of 4 instances (targets).

The following code snippets is the prometheus configuration used in the local bundle, the configuration file is at `<prometheus_repository>/prometheus/prometheus.yml`. The key-value list is very self-contained. Notice that there are different ways to specify targets: `prometheus` job claims `static_config` and lists a set of `host:port`; `node-exporter` job uses `dns_sd_config` that leverage NDS name server for services discovery. If Prometheus is running on Kubernetes cluster, there is `kubernetes_sd_config`, it leverage Kubernetes REST API. More details is available [here](https://prometheus.io/docs/prometheus/latest/configuration/configuration/).


```yml
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

## Metrics and Time Series

```shell
# HELP http_requests_total Total number of HTTP requests made.
# TYPE http_requests_total counter
http_requests_total{code="200",handler="config",method="get"} 2
http_requests_total{code="200",handler="graph",method="get"} 2
http_requests_total{code="200",handler="label_values",method="get"} 3
http_requests_total{code="200",handler="prometheus",method="get"} 2700
http_requests_total{code="200",handler="query",method="get"} 5
http_requests_total{code="200",handler="static",method="get"} 24
```

When Prometheus scrapes metrics, it requires a [text-based format](https://prometheus.io/docs/instrumenting/exposition_formats/#text-based-format). 

## Format: Comments, help text and type information
* `# <comments>`
* `# HELP <metric name> [docstring]`
* `# TYPE <metric name> [counter|gauge|histogram|untyped]`
* `metric_name [{ "label_name="label_value", ... }] value (decimal) [timestamp]`


 
Notice that there are four metric types in Prometheus: Counter, Gauge, Histogram, Summary. 

* A counter metric consists of a series of cumulative samples whose value can only increase. *** The value resets to zero if job restarts. ***
* A gauge metric represents a series of numerical value that can arbitrarily go up and down.
* Any untyped metric, untyped metic should be avoided in the most of times.
* A histogram metric usually consists of multiple correlated metrics and tightly couples with the build-in `histogram_quantile()` functions, We will discuss them in a separate post later. 


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