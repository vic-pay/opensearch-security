# Opensearch clustering

This repository contains Opensearch clustering instructions

## Usage

Add shared docker config `extends\opensearch.yml`:

```
services:
  opensearch:
    image: opensearchproject/opensearch:$OS_VERSION
    environment:
      discovery.seed_hosts: osc,osm,osd1,osd2,osd3

      cluster.name: os-cluster
      cluster.initial_master_nodes: osm

      plugins.security.allow_default_init_securityindex: 'true'
      plugins.security.allow_unsafe_democertificates: 'false'
      plugins.security.nodes_dn:        'CN=admin'
      plugins.security.authcz.admin_dn: 'CN=admin'

      plugins.security.ssl.http.enabled: 'true'
      plugins.security.ssl.http.pemkey_filepath:        certificates/key/server.key 
      plugins.security.ssl.http.pemcert_filepath:       certificates/crt/server.crt
      plugins.security.ssl.http.pemtrustedcas_filepath: certificates/crt/server.crt
      
      plugins.security.ssl.transport.enabled: 'true'
      plugins.security.ssl.transport.pemkey_filepath:        certificates/key/server.key 
      plugins.security.ssl.transport.pemcert_filepath:       certificates/crt/server.crt
      plugins.security.ssl.transport.pemtrustedcas_filepath: certificates/crt/server.crt
      plugins.security.ssl.transport.enforce_hostname_verification: 'false'

      compatibility.override_main_response_version: 'true'
      
      DISABLE_INSTALL_DEMO_CONFIG: 'true'
      JAVA_HOME: /usr/share/opensearch/jdk
      OPENSEARCH_JAVA_OPTS: "-Xms512m -Xmx512m"
      bootstrap.memory_lock: 'true'
      network.host: 0.0.0.0
    volumes:
      - type: bind
        source: ../ssl
        target:   /usr/share/opensearch/config/certificates
        read_only: true

      - type: bind
        source: ../configs/opensearch/internal_users.yml
        target:   /usr/share/opensearch/config/opensearch-security/internal_users.yml
        read_only: true
    ulimits: 
      memlock:
        soft: -1
        hard: -1
      nofile:
        soft: 262144
        hard: 262144
    restart: unless-stopped
```

Change `docker-compose.yml`:

```
services:

  # Opensearch
  opersearch-coordinator:
    extends:
      file: extends/opensearch.yml
      service: opensearch
    container_name: osc
    hostname:       osc
    environment:
      node.name: osc
      node.roles: 'cluster_manager'
    volumes:
      - type: volume
        source: opensearch-coordinator
        target: /usr/share/opensearch/data
        read_only: false
    networks:   ['opensearch']    
    ports:      ['9200:9200', '9600:9600']

  opensearch-master:
    extends:
      file: extends/opensearch.yml
      service: opensearch
    container_name: osm
    hostname:       osm
    environment:
      node.name: osm
      node.roles: 'master'
    volumes:
      - type: volume 
        source: opensearch-master
        target: /usr/share/opensearch/data
        read_only: false
    networks:   ['opensearch']

  opensearch-data-1:
    extends:
      file: extends/opensearch.yml
      service: opensearch
    container_name: osd1
    hostname:       osd1
    environment:
      node.name: osd1
      node.roles: 'ingest, data'
    volumes:
      - type: volume 
        source: opensearch-data-1
        target: /usr/share/opensearch/data
        read_only: false
    networks: ['opensearch']

  opensearch-data-2:
    extends:
      file: extends/opensearch.yml
      service: opensearch
    container_name: osd2
    hostname:       osd2
    environment:
      node.name: osd2
      node.roles: 'ingest, data'
    volumes:
      - type: volume 
        source: opensearch-data-2
        target: /usr/share/opensearch/data
        read_only: false
    networks: ['opensearch']

  opensearch-data-3:
    extends:
      file: extends/opensearch.yml
      service: opensearch
    container_name: osd3
    hostname:       osd3
    environment:
      node.name: osd3
      node.roles: 'ingest, data'
    volumes:
      - type: volume 
        source: opensearch-data-3
        target: /usr/share/opensearch/data
        read_only: false
    networks: ['opensearch']

  # Dashboards
  opensearch-dashboards:
    image: opensearchproject/opensearch-dashboards:$OS_VERSION
    container_name: osdb
    hostname:       osdb
    environment:
      SERVER_NAME: osdb
      SERVER_HOST: 0.0.0.0

      OPENSEARCH_USERNAME: kibanaserver
      OPENSEARCH_PASSWORD: a12345678

      SERVER_SSL_ENABLED: 'true'
      SERVER_SSL_KEY:         /usr/share/opensearch-dashboards/config/certificates/key/server.key
      SERVER_SSL_CERTIFICATE: /usr/share/opensearch-dashboards/config/certificates/crt/server.crt
      
      OPENSEARCH_SSL_CERTIFICATEAUTHORITIES: '["/usr/share/opensearch-dashboards/config/certificates/crt/server.crt"]'
      OPENSEARCH_SSL_VERIFICATIONMODE: certificate
                    
      OPENSEARCH_HOSTS: '["https://osc:9200","https://osm:9200","https://osd1:9200","https://osd2:9200","https://osd3:9200"]'
      DISABLE_INSTALL_DEMO_CONFIG: 'true'
    volumes:
      - type: bind
        source: ./ssl
        target:  /usr/share/opensearch-dashboards/config/certificates
        read_only: true
    networks:  ['opensearch']
    ports:     ['5601:5601']
    restart: unless-stopped

volumes:
  opensearch-coordinator:
    name: opensearch-coordinator
  opensearch-master:
    name: opensearch-master
  opensearch-data-1:
    name: opensearch-data-1
  opensearch-data-2:
    name: opensearch-data-2
  opensearch-data-3:
    name: opensearch-data-3

networks:
  opensearch:
    name: opensearch
```

Deploy conrainers:

```
docker-compose up -d
```