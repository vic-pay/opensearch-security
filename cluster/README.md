# Opensearch docker-compose

This repository contains docker-compose sample for Opensearch cluster with SSL


## Usage

1) Make **.env** file with opensearch version. Example:

        OS_VERSION=2.4.1

2) Run ansible playbook for self-signed certificate generation:

        cd opensearch-docker-compose
        ansible-playbook playbook-gen-crt.yml 

3) Deploy containers:

        docker-compose up -d

4) Login https://127.0.0.1:5601 with **admin / a12345678** credentials


## Change passwords

1) Change OPENSEARCH_PASSWORD variable in docker-compose.yml  

2) Gen new password hash

        docker exec -it osm /usr/share/opensearch/plugins/opensearch-security/tools/hash.sh
        # type new password
    
3) Change hashes in configs/internal_users.yml

## Troubleshooting 

Checl indexes and documents:

1) Check indexes in **Index Management -> Indices**
1) Create index patterns in **Stack management -> Index Patterns**
2) Check documents in **Discover**

Check clients:

        docker exec -it auditbeat auditbeat --strict.perms=false test output
        docker exec -it auditbeat auditbeat --strict.perms=false setup

        docker exec -it filebeat filebeat --strict.perms=false test output
        docker exec -it filebeat filebeat --strict.perms=false setup

Read logs:

        docker-compose logs -f