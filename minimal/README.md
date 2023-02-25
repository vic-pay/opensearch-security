# Opensearch testing enviroment

This repo contains full env for testing Opensearch 2.*:

- *beats clients
- one opensearch server
- opensearch dashboards server

## Get started

Make `.env` file. Example content:

    OS_VERSION=2.4.1

Run commands:

    docker-compose up

    docker exec -it auditbeat auditbeat --strict.perms=false test output
    docker exec -it auditbeat auditbeat --strict.perms=false setup

## API requests examples

    # Ingest pipelines
    GET /_ingest/pipeline 

    # List index template
    GET /_cat/templates
    GET /_template
    GET /_index_template

    # Lifecicle https://opensearch.org/docs/latest/im-plugin/ism/index/
    GET _plugins/_ism/policies

