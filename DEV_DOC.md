# DEV_DOC — Inception (Developer Documentation)

This document explains how a developer can set up, build, run, and maintain the Inception project.

---

## 1. Prerequisites

Required:
- Docker
- Docker Compose (v2)
- GNU Make

Recommended:
- Linux environment compatible with 42 requirements
- `curl` and `openssl` for basic checks

---

## 2. Repository structure

```text
.
├── Makefile
├── README.md
├── USER_DOC.md
├── DEV_DOC.md
├── srcs
│   ├── docker-compose.yml
│   └── requirements
│       ├── mariadb
│       │   ├── Dockerfile
│       │   ├── conf
│       │   │   └── my.cnf
│       │   └── tools
│       │       └── entrypoint.sh
│       ├── nginx
│       │   ├── Dockerfile
│       │   ├── conf
│       │   │   └── nginx.conf
│       │   └── tools
│       │       └── gen-cert.sh
│       └── wordpress
│           ├── Dockerfile
│           ├── conf
│           │   └── php-fpm.conf
│           └── tools
│               └── entrypoint.sh
└── secrets (stored outside the repository)
```

## 3. Environment configuration (.env)

The `.env` file centralizes variables used by Docker Compose.
No passwords or sensitive values are stored in this file.

Main variables used by the project:

- `DOMAIN_NAME=nryser.42.fr`
- `MYSQL_DATABASE=wordpress`
- `MYSQL_USER=wpuser`
- `WP_ADMIN_USER=nryser42`
- `WP_ADMIN_EMAIL=nryser@student.42lausanne.ch`
- `WP_AUTO_INSTALL=1`
- `LOGIN=nryser`
- `WP_SECOND_USER=writer42`
- `WP_SECOND_EMAIL=writer42@example.com`

Secrets are stored outside the repository and referenced using:
- `SECRETS_DIR=/home/${LOGIN}/.inception_secrets`

---

## 4. Secrets management

Sensitive data is handled using Docker secrets and stored outside the repository.

Secrets directory:
- `/home/nryser/.inception_secrets`

Required secret files:
- `db_root_password.txt`
- `db_password.txt`
- `credentials.txt`
- `wp_second_pass.txt`

Recommended permissions:
```bash
chmod 700 /home/nryser/.inception_secrets
chmod 600 /home/nryser/.inception_secrets/*.txt
```

Inside containers, secrets are available at:
- `/run/secrets/db_root_password`
- `/run/secrets/db_password`
- `/run/secrets/credentials`
- `/run/secrets/wp_second_pass`

---

## 5. Build and launch the project

From the project root directory:

### Build and start all services
```bash
make up
```

### Build images only
```bash
make build
```

### Stop containers (keep persistent data)
```bash
make down
```

### Restart services
```bash
make restart
```

### View logs
```bash
make logs
```

### Check status
```bash
make ps
```
## 6. Container and volume management

### List running containers
```bash
docker ps
```

### Inspect Docker Compose state
```bash
docker compose -f srcs/docker-compose.yml --env-file .env ps
```

### Follow logs using Docker Compose
```bash
docker compose -f srcs/docker-compose.yml --env-file .env logs -f
```
### Execute a shell inside a container
```bash
docker exec -it <container_name> sh
```

## 7. Data persistence

This project uses bind mounts to persist data on the host.

Data directories are created using:
- `LOGIN=nryser`

Typical paths:
- `/home/nryser/data/mariadb`
- `/home/nryser/data/wordpress`

To verify the exact mounts used by a container:
```bash
docker inspect <container_name> | grep -A3 Mounts
```

### Reset behavior
- `make down` removes containers but keeps bind mount data.
- Reset targets may delete data directories and remove all content.

---

## 8. Validation and debugging

### HTTPS validation
```bash
curl -kI https://nryser.42.fr
```

### WordPress response
```bash
curl -k https://nryser.42.fr | head
```

### WordPress administration panel
Open:
- https://nryser.42.fr/wp-admin

---

## 9. Developer notes

- Never hardcode passwords in Dockerfiles.
- Keep initialization logic inside entrypoint scripts.
- Ensure correct startup order: MariaDB → WordPress → NGINX.
- Keep secrets out of git and enforce strict file permissions.



