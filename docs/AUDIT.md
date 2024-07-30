# Opensearch audit

This document contains Opensearch audit settings

## Usage

Add setting to environment:

    plugins.security.audit.type: internal_opensearch

(Not necessary) Set audit in `/usr/share/opensearch/config/opensearch-security/audit.yml`

```
volumes:
  - type: bind
    source: ./configs/opensearch/audit.yml
    target:  /usr/share/opensearch/config/opensearch-security/audit.yml
    read_only: true
```