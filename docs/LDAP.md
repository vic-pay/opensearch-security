# LDAP authentication in Opensearch

This repository contains instructions for setting up LDAP authentication Opensearch

Configs are stored in:

* configs/opensearch/config.yml
* configs/opensearch/roles_mapping.yml

## Usage

Set up `.env` file. Example:

```
LDAP_HOST='ldap-server.domain.com:636'
LDAP_USER='CN=ldap-user,OU=users,DC=domain,DC=com'
LDAP_PASS='ldap-password'
LDAP_USER_BASE='OU=users,DC=domain,DC=com'
LDAP_ROLE_BASE='OU=roles,DC=domain,DC=com'
```

Copy CA certificate to `ssl/crt/ca_ldap.crt` file

Deploy containers:

```
docker-compose up -d
```

## Troubleshooting

Settings doesn't apply - delete old files:

    docker-compose down -v



