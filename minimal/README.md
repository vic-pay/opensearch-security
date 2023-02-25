# Opensearch testing enviroment

This repo contains full env for testing Opensearch 2.*:

- *beats clients
- one opensearch server
- opensearch dashboards server

## Get started

Run commands:

    docker-compose up

    docker exec -it auditbeat auditbeat --strict.perms=false test output
    docker exec -it auditbeat auditbeat --strict.perms=false setup

## API requests

    # Ingest pipelines
    GET /_ingest/pipeline 

    # List index template
    GET /_cat/templates
    GET /_template
    GET /_index_template

    # Lifecicle https://opensearch.org/docs/latest/im-plugin/ism/index/
    GET _plugins/_ism/policies


## Testing results

Sources and storage
- [x] beats support - only 7.10.2 with ingest pipelines or 7.12.1 without
- [x] ingest pipelines - but without grafic editor
- [x] index templates - cant display, only API list 
- [x] index lifetime policies
- [x] data streams

Notifications: 
- slack 
- chime
- custom webhook
- email
- amazon SNS

Rules
types - only sigma 
[security_analytics_exception] Limit of total fields [1000] has been exceeded
https://github.com/opensearch-project/security-analytics/issues/179