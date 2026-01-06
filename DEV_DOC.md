# DEV_DOC — Inception (Developer Documentation)

This document explains how to set up, build, run, and maintain the project as a developer.

---

## 1. Prerequisites

Install:

- Docker
- Docker Compose (v2)
- GNU Make

Recommended:
- Linux or macOS environment
- Sudo access (depending on Docker installation)
- `curl` / `openssl` for quick checks

---

## 2. Repository structure (typical)

This project usually contains:

- `Makefile`
- `docker-compose.yml` (or `srcs/docker-compose.yml`)
- `requirements/` (service Dockerfiles + configs)
  - `requirements/nginx/`
  - `requirements/wordpress/`
  - `requirements/mariadb/`
- `.env`
- `secrets/` (Docker secrets) or `srcs/secrets/`
- Persistent data bind mounts (often under):
  - `/home/<user>/data/...` (42 common approach)
  - or `data/` inside the repo (less common for 42)

If your structure differs, adjust the paths in this doc.

---

## 3. Configuration (.env) and secrets

### Environment variables
The `.env` file typically defines values such as:
- `DOMAIN_NAME`
- `WP_TITLE`
- `WP_ADMIN_USER`, `WP_ADMIN_EMAIL`
- `DB_NAME`, `DB_USER`

### Secrets
Sensitive values should be stored in Docker secret files, for example:
- database password
- database root password
- WordPress admin password

Typical pattern:
- Compose references secret files
- Containers read secrets from `/run/secrets/<secret_name>`

Example inside a container:
```bash
cat /run/secrets/db_password
