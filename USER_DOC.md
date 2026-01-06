# USER_DOC — Inception (User / Admin Documentation)

This document explains how to use the Inception stack as an end user or administrator.

---

## 1. What services are provided?

This project deploys a WordPress website using Docker Compose with 3 main services:

- **NGINX**
  - Public entrypoint of the stack
  - Serves the website over **HTTPS**
  - Forwards PHP requests to WordPress (PHP-FPM)

- **WordPress (PHP-FPM)**
  - Runs the WordPress application
  - Provides the **website** and the **WordPress admin panel**

- **MariaDB**
  - Stores WordPress data (users, posts, settings, etc.)

Only **NGINX** is exposed publicly. WordPress and MariaDB communicate internally over the Docker network.

---

## 2. Start and stop the project

From the root of the repository:

### Start
```bash
make up
```

### Stop (remove containers, keep persistent data)
```bash
make down
```
### Restart
```bash
make restart
```
### View logs
```bash
make logs
```
### Show container status
```bash
make ps
```


