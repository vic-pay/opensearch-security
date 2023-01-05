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