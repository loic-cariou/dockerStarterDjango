# Loïc Cariou Docker starter python

## Introduction

Copy private SSH key in .docker/frontend/ssh

### How to add Elasticsearch and Kibana

<details>

<summary>Read the cookbook</summary>

In order to use Elasticsearch and Kibana, you should add the following content
to the `docker-compose.yml` file:

```yaml
volumes:
    elasticsearch-data: {}

services:
    elasticsearch:
        image: elasticsearch:7.3.2
        volumes:
            - elasticsearch-data:/usr/share/elasticsearch/data
        environment:
            - "ES_JAVA_OPTS=-Xms128m -Xmx128m"
            - "discovery.type=single-node"

    kibana:
        image: kibana:7.3.2
        depends_on:
            - elasticsearch
```

Then, you will be able to browse:

* `https://kibana.<root_domain>`
* `https://elasticsearch.<root_domain>`

</details>

### How to add RabbitMQ and its dashboard

<details>

<summary>Read the cookbook</summary>

In order to use RabbitMQ and its dashboard, you should add the following content
to the `docker-compose.yml` file:

```yaml
volumes:
    rabbitmq-data: {}

services:
    rabbitmq:
        image: rabbitmq:3-management-alpine
        volumes:
            - rabbitmq-data:/var/lib/rabbitmq
        environment:
            - "RABBITMQ_VM_MEMORY_HIGH_WATERMARK=1024MiB"
```

In order to publish and consume messages with PHP, you need to install the
`php${PHP_VERSION}-amqp` in the `php-base` image.

Then, you will be able to browse:

* `https://rabbitmq.<root_domain>`

</details>

### How to add Redis and its dashboard

<details>

<summary>Read the cookbook</summary>

In order to use Redis and its dashboard, you should add the following content to
the `docker-compose.yml` file:

```yaml
volumes:
    redis-data: {}
    redis-insight-data: {}

services:
    redis:
        image: redis:5
        volumes:
            - "redis-data:/data"

    redis-insight:
        image: redislabs/redisinsight
        volumes:
            - "redis-insight-data:/db"

```

In order to communicate with Redis, you need to install the
`php${PHP_VERSION}-redis` in the `php-base` image.

Then, you will be able to browse:

* `https://redis.<root_domain>`

</details>

### How to add Maildev

<details>

<summary>Read the cookbook</summary>

In order to use Maildev and its dashboard, you should add the following content
to the `docker-compose.yml` file:

```yaml
services:
    maildev:
        image: djfarrelly/maildev
        command: ["bin/maildev", "--web", "80", "--smtp", "25", "--hide-extensions", "STARTTLS"]
```

Then, you will be able to browse:

* `https://maildev.<root_domain>`

</details>

### How to add Mercure

<details>

<summary>Read the cookbook</summary>

In order to use Mercure, you should add the following content to the
`docker-compose.yml` file:

```yaml
services:
    mercure:
        image: dunglas/mercure
        environment:
            - "JWT_KEY=password"
            - "ALLOW_ANONYMOUS=1"
            - "CORS_ALLOWED_ORIGINS=*"
```

If you are using Symfony, you must put the following configuration in the `.env` file:

```
MERCURE_PUBLISH_URL=http://mercure/.well-known/mercure
MERCURE_JWT_TOKEN=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJtZXJjdXJlIjp7InN1YnNjcmliZSI6W10sInB1Ymxpc2giOltdfX0.t9ZVMwTzmyjVs0u9s6MI7-oiXP-ywdihbAfPlghTBeQ
```

</details>
