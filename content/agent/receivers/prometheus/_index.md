---
title: "Prometheus"
date: 2019-02-13T11:40:11-08:00
logo: /img/prometheus-logo.png
---

- [Introduction](#introduction)
- [Configuration](#configuration)
    - [Format](#format)
    - [Example](#example)
- [References](#references)

### Introduction

ocagent can scrape your applications for stats, just like Prometheus traditionally does.

This makes ocagent a drop-in replacement for Prometheus' scrapers but with the added
advantage that now your stats from all sorts of applications can be converted into
OpenCensus Metrics and exported to diverse exporters.

### Configuration

Following Prometheus' configuration file guide at [Prometheus Configuration](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)

Instead of starting Prometheus with a configuration file, copy the contents of that configuration file.

#### Format

Paaste and indent them under sub-section
```yaml
receivers:
  prometheus:
    config:
      <the_configuration_in_your_prometheus_config_file.yaml_goes_here>
```

#### Example
So for an example YAML file comparing "After" and "Before":

{{<tabs After Before>}}
{{<highlight yaml>}}
receivers:
  prometheus:
    config:
      global:
        scrape_interval: 15s
        scrape_timeout: 8s
        external_labels:
          deployment: ml
          api: v2

      scrape_configs:
        - job_name: 'jdbc_pool'
          scrape_interval: 5s
          static_configs:
            - targets: ['localhost:8889']

        - job_name: 'memcached'
          scrape_interval: 8s
          static_configs:
            - targets: ['localhost:9977']
{{</highlight>}}

{{<highlight yaml>}}
global:
  scrape_interval: 15s
  scrape_timeout: 8s
  external_labels:
    deployment: ml
    api: v2

scrape_configs:
  - job_name: 'jdbc_pool'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:8889']

  - job_name: 'memcached'
    scrape_interval: 8s
    static_configs:
      - targets: ['localhost:9977']
{{</highlight>}}
{{</tabs>}}

And then run the ocagent normally like you would
```shell
$ ./bin/ocagent_darwin --config prom.yaml 
2019/02/13 15:02:15 Running zPages at ":55679"
2019/02/13 15:02:15 Running Prometheus receiver
```

### References

Resource|URL
---|---
Prometheus project home|https://prometheus.io/
Prometheus Configuration|[Configuration docs](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)
