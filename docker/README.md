# Nginx Docker container with TLS  Encryption enabled
Deploy Nginx Docker container with self signed TLS Certificate using Docker Compose
```
docker compose up -d
```

Docker Compose file container Image Build ARGS which can be overriden to generate self signed TLS/SSL certificates usign openssl. Build ARGS :

- COUNTRY
- STATE
- LOCATION
- ORGANIZATION
- COMMON_NAME