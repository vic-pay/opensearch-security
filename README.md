# Opensearch security

This repository contains docker-compose sample for learning Opensearch security

## Usage

Generate SSL certificates with ansible playbook:

    ansible-playbook playbook_gen_crt.yml

Deploy containers with `docker-compose`:

    docker-compose up -d

Log in to OpenSearch Dashboars https://localhost:5601 with **admin/a12345678** credentials

## Security topics

* [SIGMA rules](docs/SIGMA.md)
* [LDAP authentication](docs/LDAP.md)

## Customize deployment

* [Opensearch cluster](docs/CLUSTERING.md)
* [Opensearch monitoring](docs/MONITORING.md)