*This project has been created as part of the 42 curriculum by nryser.*

# Inception

## Description
Inception is a system-administration and DevOps project focused on building a complete
web stack using **Docker** and **Docker Compose**, without relying on prebuilt images.

The project deploys a **WordPress website** served over **HTTPS** using:
- **NGINX** as a reverse proxy and web server
- **PHP-FPM** to execute WordPress PHP code
- **MariaDB** as the relational database

Each service runs in its **own container**, connected through a dedicated Docker network,
and configured according to best practices regarding security, persistence, and isolation.

The main goal is to understand how containers work, how services communicate,
and how to properly manage configuration, secrets, and data persistence.

---

## Project Architecture

Services:
- **NGINX**
  Serves WordPress over TLS (HTTPS only) and forwards PHP requests to PHP-FPM.
- **WordPress (PHP-FPM)**
  Executes WordPress PHP code and handles application logic.
- **MariaDB**
  Stores WordPress data (users, posts, configuration).

Key features:
- HTTPS only (TLS 1.2 / 1.3)
- No host networking, no `--link`
- No infinite loops or hacky keep-alive commands
- Persistent data using bind mounts
- Secrets handled via Docker secrets
- Staged startup using health checks

---

## Instructions

### Requirements
- Docker
- Docker Compose (v2)
- GNU Make

### Setup
1. Clone the repository:
   ```bash
   git clone <repo_url>
   cd inception
