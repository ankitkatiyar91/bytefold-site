---
layout: post
title: Superset on docker
date: 2022-07-24 00:56:25.000000000 +05:30
type: post
parent_id: '0'
published: true
categories:
- Data Visualization
- Docker

tags: [Apache Superset, Data Visualization]
author: ankit_katiyar
permalink: "/running-superset-on-docker/"
---

Apache Superset is great tool for data visualization which comes with many useful features. It's built using python
flask and JS frameworks. You can deploy it for your use case in multiple ways. I will share with you how to deploy it on
docker and do the necessary steps to make it work for you.

Apache superset provids an official docker image available on
Dockerhub [https://hub.docker.com/r/apache/superset](https://hub.docker.com/r/apache/superset) that
can be used to run superset but this image lacs few functionality which are critical to use it in production
environment.

### Missing components from official Apache Superset docker image:

* User Login
* Database Drivers
* Certificates and other dependencies

#### User Login

Superset image form dockerhub does not provide any easily accessible way to configure Admin user for the app. It relies
on superset `fab create-admin` command to create admin user. We often do not have acess to perform these tasks manually
from a docker container (specially when running on a managed cloud service like AWS ECS).

#### Database Drivers

Apache Superset supports a wide range of databases for visualization. Official dockerhub image keeps it light by not
addd all the required drivers and letting you add them later. You can find driver for your need in the
list [https://superset.apache.org/docs/databases/installing-database-drivers/](https://hub.docker.com/r/apache/superset)
here.

#### Certificates and other dependencies

Superset supports wide range of databases and services to be integated with and lot of these services support using
custom SSL Certificates to be used for the communication. Apache Superset does not comes with intutive user interface to
add these certificates and other dependencies. It has added few features in recent version to add ROOT CA from UI still
sometimes it's easier to have these dependencies baked in before you run it.

### Additional Configuration Support

Superset uses python file named `superset_config.py` to customize a lot of superset functionality. You can set many
variables which are used in Superset for enabling/disabling features. Below are few examples of that are

##### Enabling Jinja template

```python
FEATURE_FLAGS = {
    "ENABLE_TEMPLATE_PROCESSING": True,
}
```

##### Enabling Proxy Fix for SSL support

```python
ENABLE_PROXY_FIX = True
```

### Creating Custom Docker Image

Simplest solution to these problems is creating your own custom docker image. You can do it form Apache Superset
source [https://github.com/apache/superset](https://hub.docker.com/r/apache/superset) or by extending official image.
You can add more functionalites by adding custom config files and unix scripts that can we run an entrypoint (start of
the docker container).

**superset-custom.sh**

```shell
#!/bin/bash

# create Admin user, you can read these values from env or anywhere else possible.
echo "Creating admin user ${ADMIN_USERNAME} email ${ADMIN_EMAIL}"
superset fab create-admin --username "$ADMIN_USERNAME" --firstname Superset --lastname Admin --email "$ADMIN_EMAIL" --password "$ADMIN_PASSWORD"

# Upgrading Supersetset metastore.
echo "Upgrading DB"
superset db upgrade

echo "Setup roles"
superset superset init  #setup roles and permissions

echo "Starting server"
/bin/sh -c /usr/bin/run-server.sh
```

**Dockerfile**

```dockerfile
# Define base image
FROM apache/superset:1.5.1

# Switching to root to install the required packages
USER root

# Intall required dependencies.
# Example: installing the MySQL driver to connect to the metadata database
# if you prefer Postgres, you may want to use `psycopg2-binary` instead
RUN pip install psycopg2

# Define additional environment variables to be used 
ENV ADMIN_EMAIL admin@superset.com

# Add custom superset_config.py file and shell files
COPY superset_config.py /app/
ENV SUPERSET_CONFIG_PATH /app/superset_config.py #define variable that is used by Superset to look for config python file.

COPY superset-init.sh /app/superset-custom.sh
RUN chmod +x /app/superset-custom.sh

# Switching back to using the `superset` user
USER superset
ENTRYPOINT [ "/app/superset-custom.sh" ]

```

You can add a lot more to your docker image and customize it for you purpose. I hope it will help you starting with your
Apache Superset Journey

