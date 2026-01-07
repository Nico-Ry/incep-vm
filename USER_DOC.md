# USER_DOC — Inception (User / Admin Documentation)

This document explains how to use the Inception stack as an end user or administrator.

---

## 1. Services provided by the stack

This project deploys a WordPress website using Docker Compose with 3 main services:

- **NGINX**
  - Public entrypoint of the stack
  - Serves the website over **HTTPS**
  - Forwards PHP requests internally to WordPress (**PHP-FPM**)

- **WordPress (PHP-FPM)**
  - Runs the WordPress application
  - Provides the website and the WordPress administration panel

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

## 3. Access the website and the administration panel

The domain is defined in the `.env` file:

- `DOMAIN_NAME=nryser.42.fr`

### Website
Open in a browser:
- https://nryser.42.fr

### WordPress administration panel
Open:
- https://nryser.42.fr/wp-admin

Log in using the WordPress administrator credentials configured for the project.

---

## 4. Locate and manage credentials

This project uses environment variables (non-secret identifiers) and Docker secrets (passwords).

### Non-secret identifiers (stored in `.env`)
- `DOMAIN_NAME`
- `MYSQL_DATABASE`
- `MYSQL_USER`
- `WP_ADMIN_USER`
- `WP_ADMIN_EMAIL`
- `WP_SECOND_USER`
- `WP_SECOND_EMAIL`
- `LOGIN`

### Secrets location (outside the repository)

Secrets are stored outside the repository for security reasons.

The location is defined in `.env` using:
- `SECRETS_DIR=/home/${LOGIN}/.inception_secrets`

For this project:
- `LOGIN=nryser`
- Secrets directory: `/home/nryser/.inception_secrets`

### Required secret files

The following files must exist in the secrets directory:

- `db_root_password.txt`  — MariaDB root password
- `db_password.txt`       — MariaDB user password
- `credentials.txt`       — WordPress admin password
- `wp_second_pass.txt`    — WordPress second user password

### Recommended permissions

```bash
chmod 700 /home/nryser/.inception_secrets
chmod 600 /home/nryser/.inception_secrets/*.txt
 ```

### 5. Check that the services are running correctly

### Container status
```bash
make ps

You should see the following containers running:
- `nginx`
- `wordpress`
- `mariadb`

### Logs
```bash
make logs
```

### HTTPS check
```bash
curl -kI https://nryser.42.fr
```

### WordPress administration panel
Open:
- https://nryser.42.fr/wp-admin

---

## 6. Troubleshooting

### Website does not load
1. Check container status:
```bash
make ps
```
2. Check logs:
```bash
make logs
```
### WordPress database connection error
- MariaDB may still be initializing on first startup.
- Verify that database credentials stored in Docker secrets match the configuration.
- Check MariaDB logs for errors.

### Reset targets
The Makefile provides reset commands (for example: `make wp-reset`, `make db-reset`, `make re`, `make pristine`),
be aware that they may delete persistent data.



