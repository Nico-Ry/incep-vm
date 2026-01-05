*This project has been created as part of the 42 curriculum by nryser.*

# Inception

## Table of Contents
- [Description](#description)
- [Project Architecture](#project-architecture)
- [Services](#services)
- [Key Features](#key-features)
- [Instructions](#instructions)
- [Requirements](#requirements)
- [Setup](#setup)
- [Resources](#resources)

---

## Description

**Inception** is a system-administration and DevOps project focused on building a complete
web stack using **Docker** and **Docker Compose**, without relying on pre-built images.

The project deploys a **WordPress website** served exclusively over **HTTPS**, using:

- **NGINX** as a reverse proxy and web server
- **PHP-FPM** to execute WordPress PHP code
- **MariaDB** as the relational database

Each service runs in its **own container**, connected through a dedicated Docker network
and configured according to best practices regarding **security**, **persistence**, and
**service isolation**.

The main goal of this project is to understand how containers work, how services
communicate with each other, and how to properly manage configuration, secrets, and data
persistence in a production-like environment.

---

## Project Architecture

![Docker WordPress Architecture](https://raw.githubusercontent.com/Xperaz/inception-42/main/assets/inception_architecture.png)

The infrastructure consists of three core services orchestrated via
**Docker Compose** and started in a controlled order using **health checks**.

All external traffic is handled by **NGINX**, which terminates TLS connections and forwards
PHP requests internally to **PHP-FPM**. WordPress communicates with **MariaDB** over an
internal Docker network.

No service is directly exposed to the host except NGINX.

---

## Services

### NGINX
- Serves the website over HTTPS only
- Enforces TLS 1.2 / TLS 1.3
- Acts as a reverse proxy
- Forwards PHP requests to PHP-FPM via FastCGI

### WordPress (PHP-FPM)
- Executes WordPress PHP code
- Handles application logic
- Installed and configured using `wp-cli`
- Uses environment variables and Docker secrets

### MariaDB
- Stores WordPress data (users, posts, configuration)
- Uses persistent bind mounts
- Securely initialized on first startup

---

## Key Features

- HTTPS only (TLS 1.2 / TLS 1.3)
- No host networking
- No `--link`
- No infinite loops or fake keep-alive commands
- Persistent data using bind mounts
- Secrets handled via Docker secrets
- Staged startup using health checks
- Clean shutdown and restart behavior

---

## Instructions

Available `Makefile` commands:

```text
make up              : Build (if needed) and start services (DB → WP → NGINX)
make build           : Build images only
make down            : Stop and remove containers (keeps bind directories)
make restart         : Restart all services (staged)
make logs            : Follow logs for all services
make ps              : Show container status

make sh-db           : Shell into MariaDB container
make sh-wp           : Shell into WordPress container
make sh-nginx        : Shell into NGINX container

make cert-re         : Rebuild NGINX and recreate certificates
make env             : Show resolved compose configuration

make wp-reset        : Delete WordPress data and restart (DB preserved)
make db-reset        : Delete database data and restart (content lost)
make re              : Full reset (DB + WP data)
make pristine        : Absolute reset (volumes + images)

make wp-purge-sample : Remove default WordPress sample content
make test            : Infrastructure smoke tests
make verify-reset    : Show bind mounts and current state
```

## Requirements

- Docker
- Docker Compose (v2)
- GNU Make

---

## Setup

1. Clone the repository:
   ```bash
   git clone <repo_url>
   cd inception
   ```

2. Build and start the infrastructure:
   ```bash
   make up
   ```

3. Access the website:
   https://localhost

## Resources
### General Docker & Containers
- [What is Docker and how it works](https://devopscube.com/what-is-docker/)
- [Cgroups, namespaces, and containers explained](https://www.youtube.com/watch?v=sK5i-N34im8)
- [Docker Tutorial for Beginners](https://www.youtube.com/watch?v=zJ6WbK9zFpI)
- [Docker Containers 101](https://youtu.be/eGz9DS-aIeY)
- [18 Weird and Wonderful Ways I Use Docker](https://youtu.be/RUqGlWr5LBA)

### Docker Networking
- [Explaining Docker networking concepts](https://ostechnix.com/explaining-docker-networking-concepts/)
- [Why containers sometimes cannot communicate](https://tomd.xyz/why-containers-wont-talk/)
- [Docker networking is CRAZY](https://youtu.be/bKFMS5C4CG0)

### Web Stack
- [WordPress with NGINX, PHP-FPM and MariaDB](https://medium.com/swlh/wordpress-deployment-with-nginx-php-fpm-and-mariadb-using-docker-compose-55f59e5c1a)
- [Install WordPress using wp-cli](https://blog.sucuri.net/2022/11/wp-cli-how-to-install-wordpress-via-ssh.html)
- [Learn CGI and FastCGI](https://www.howtoforge.com/install-adminer-database-management-tool-on-debian-10/)

### Security
- [Configure NGINX to use TLS 1.2 / 1.3 only](https://www.cyberciti.biz/faq/configure-nginx-to-use-only-tls-1-2-and-1-3/)

### MariaDB
- [How to install MariaDB on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-ubuntu-20-04)

### Inspiration
- [Xperaz – Inception README](https://github.com/Xperaz/inception-42)
- [42 Inception tutorial](https://tuto.grademe.fr/inception/)

