# Introduction

Dockerfile to build an [Activiti BPM](#http://www.activiti.org/) container image.

## Version

Current Version: 5.22.0

# Installation

Pull the latest version of the image from the docker index. This is the recommended method of installation as it is easier to update image in the future. These builds are performed by the **Docker Trusted Build** service.

```bash
docker pull sermojohn/rpi-activiti:latest
```

Alternately you can build the image yourself.

```bash
git clone https://github.com/sermojohn/docker-rpi-activiti.git
cd docker-rpi-activiti
docker build --tag="$USER/activiti" .
```

# Quickstart

Run the activiti image

```bash
docker run --name='activiti' -it --rm \
-p 8080:8080 \
-v /var/run/docker.sock:/run/docker.sock \
-v $(which docker):/bin/docker \
sermojohn/rpi-activiti:latest
```
# Configuration

## Database

Activiti uses a database backend to store its data. You can only configure this image to use MySQL.

### MySQL

This image can only be configured to use an external MySQL database instead of starting a MySQL server internally. The database configuration should be specified using environment variables while starting the Activiti image.

Before you start the Activiti image create user and database for activiti.

```sql
CREATE USER 'activiti'@'%.%.%.%' IDENTIFIED BY 'password';
CREATE DATABASE IF NOT EXISTS `activiti_production` DEFAULT CHARACTER SET `utf8` COLLATE `utf8_unicode_ci`;
GRANT ALL PRIVILEGES ON `activiti_production`.* TO 'activiti'@'%.%.%.%';
```

We are now ready to start the Activiti application.

*Assuming that the mysql server host is 192.0.2.1*

```bash
docker run --name=activiti -d \
  -e 'DB_HOST=192.0.2.1’ -e 'DB_NAME=activiti_production' -e 'DB_USER=activiti’ -e 'DB_PASS=password' \
sermojohn/rpi-activiti:latest
```

### Available Configuration Parameters

*Please refer the docker run command options for the `--env-file` flag where you can specify all required environment variables in a single file. This will save you from writing a potentially long docker run command.*

Below is the complete list of available options that can be used to customize your activiti installation.

- **TOMCAT_ADMIN_USER**: Tomcat admin user name. Defaults to `admin`.
- **TOMCAT_ADMIN_PASSWORD**: Tomcat admin user password. Defaults to `admin`.
- **DB_HOST**: The database server hostname. Defaults to ``.
- **DB_PORT**: The database server port. Defaults to `3306`.
- **DB_NAME**: The database database name. Defaults to ``.
- **DB_USER**: The database database user. Defaults to ``.
- **DB_PASS**: The database database password. Defaults to ``.

# References

* http://activiti.org/
* http://github.com/Activiti/Activiti
* http://tomcat.apache.org/
* http://dev.mysql.com/downloads/connector/j/5.1.html
* https://github.com/jpetazzo/nsenter
* https://jpetazzo.github.io/2014/03/23/lxc-attach-nsinit-nsenter-docker-0-9/
