
# Contents


1. [Backend](#backend)
    - 1.1 [DB(PostgreSQL)](#postgresql)
    - 1.2 [API(PostgREST)](#api)
    - 1.2 [Docker](#docker)
2. [FrontEnd(Svelte)](#frontend)

# Backend

| API | DB | API/DB UI |  
|-----------|-----------|-----------|  
| PostgREST | PostgreSQL | Swagger/PgAdmin |  



## PostgreSQL

postgresql.conf
```shell
# DB Version: 14
# OS Type: linux
# DB Type: web
# Total Memory (RAM): 32 GB
# CPUs num: 6
# Connections num: 20
# Data Storage: ssd

max_connections = 20
shared_buffers = 8GB
effective_cache_size = 24GB
maintenance_work_mem = 2GB
checkpoint_completion_target = 0.9
wal_buffers = 16MB
default_statistics_target = 100
random_page_cost = 1.1
effective_io_concurrency = 200
work_mem = 139810kB
min_wal_size = 1GB
max_wal_size = 4GB
max_worker_processes = 6
max_parallel_workers_per_gather = 3
max_parallel_workers = 6
max_parallel_maintenance_workers = 3
```

## API
[PostgREST Docker Compose](https://postgrest.org/en/stable/install.html#docker)

## Docker
All Backend is built with Docker.

<center>dockercompose.yml</center>

```docker
version: '3.1'

services:
  swagger: # API UI
    image: swaggerapi/swagger-ui
    ports:
      - "8080:8080"
    expose:
      - "8080"
    environment:
      API_URL: "http://localhost:3000"
  server: # API
    image: postgrest/postgrest
    ports:
      - "3000:3000"
    environment:
      PGRST_DB_URI: postgres://dabd:dabd@db:5432/dabd
      PGRST_DB_SCHEMA: public
      PGRST_DB_ANON_ROLE: dabd
      PGRST_OPENAPI_SERVER_PROXY_URI: http://127.0.0.1:3000
    depends_on:
      - db
  db:
    image: postgres:latest
    environment:
      - POSTGRES_USER=dabd
      - POSTGRES_PASSWORD=dabd
      - POSTGRES_DB=svelt
    ports:
      - "5432:5432"
    volumes:
      - postgresdb:/var/lib/postgresql/data
  pgadmin:
    container_name:  pgadmin4_container
    image: dpage/pgadmin4
    restart: always
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@admin.com
      PGADMIN_DEFAULT_PASSWORD: dabd
      
    ports:
      - "5050:80"
    
volumes:
  postgresdb:
    driver: local
```





# Frontend

## Svelte
