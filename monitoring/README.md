# Opensearch monitoring docker-compose

This repository contains docker-compose sample for Opensearch monitoring via Promethues and Telegraf exporter


## Usage

1) Make **.env** file with opensearch version. Example:

        OS_VERSION=2.4.1

2) Deploy containers:

        docker-compose up -d

3) Login https://127.0.0.1:3000 with **admin / admin** credentials

4) Add [Prometheus](https://grafana.com/docs/grafana/latest/datasources/prometheus/) datasource