# Opensearch monitoring

This repository contains Opensearch monitoring instruction via Telegraf with Prometheus exporter

## Usage

Add Telegraf in `docker-compose.yml`. Example:

```
telegraf:
  image: telegraf:1.23.4-alpine
  container_name: telegraf
  hostname:       telegraf
  volumes:
    - type: bind
      source: ./configs/telegraf/telegraf.conf
      target:  /etc/telegraf/telegraf.conf
      read_only: true
  depends_on: ['opensearch-min']
  networks:   ['opensearch-min']
  ports:      ['9273:9273']
  restart: always
```

Add Telegraf `./configs/telegraf/telegraf.conf` config. Example:

```
[global_tags]

[agent]
  interval = "60s"
  round_interval = true
  metric_batch_size = 1000
  metric_buffer_limit = 10000
  collection_jitter = "0s"
  flush_interval = "10s"
  flush_jitter = "0s"
  precision = ""
  hostname = "telegraf"
  omit_hostname = false

# https://github.com/influxdata/telegraf/blob/release-1.25/plugins/inputs/elasticsearch/README.md
[[inputs.elasticsearch]]
  servers      = ["https://opensearch-min:9200"]
  http_timeout = "10s"
  local          = true
  cluster_health = true
  cluster_stats  = true
  cluster_stats_only_from_master = true
  indices_include = ["_all"]
  indices_level   = "shards"
  username        = "admin"
  password        = "admin"
  insecure_skip_verify = true

# https://github.com/influxdata/telegraf/blob/release-1.25/plugins/outputs/prometheus_client/README.md
[[outputs.prometheus_client]]
  listen = ":9273"
```

Deploy containers:

```
docker-compose up -d
```

## Verification

Check metrics with `curl`:

```
curl -v http://localhost:9273/metrics
```