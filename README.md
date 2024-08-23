
# WordPress + phpMyAdmin Docker Compose

## Services

- **db**: A MariaDB container that serves as the database for the WordPress installation.
- **wordpress**: A WordPress container that hosts the WordPress site.
- **phpmyadmin**: A phpMyAdmin container for managing the MariaDB database via a web interface.

## Docker Compose Configuration

### db

- **Image**: `mariadb:latest`
- **Container Name**: `wp_mariadb`
- **Restart Policy**: Always
- **Volumes**: Stores database data persistently.
  - `db_data:/var/lib/mysql`
- **Environment Variables**:
  - `MYSQL_ROOT_PASSWORD`: password
  - `MYSQL_DATABASE`: wordpress
  - `MYSQL_USER`: wordpress
  - `MYSQL_PASSWORD`: wordpress
- **Networks**: Connected to the `wordpress` network.

### wordpress

- **Depends On**: `db`
- **Image**: `wordpress:latest`
- **Container Name**: `wordpress`
- **Restart Policy**: Always
- **Volumes**: Mounts the current directory to the WordPress web root.
  - `./:/var/www/html`
- **Environment Variables**:
  - `WORDPRESS_DB_HOST`: db:3306
  - `WORDPRESS_DB_USER`: wordpress
  - `WORDPRESS_DB_PASSWORD`: wordpress
- **Ports**: 
  - Exposes port `80` for accessing the WordPress site.
- **Networks**: Connected to the `wordpress` network.

### phpmyadmin

- **Depends On**: `db`
- **Image**: `phpmyadmin/phpmyadmin:latest`
- **Container Name**: `wordpress_phpmyadmin`
- **Restart Policy**: Always
- **Environment Variables**:
  - `PMA_HOST`: db
  - `MYSQL_ROOT_PASSWORD`: password
- **Ports**:
  - Exposes port `8080` for accessing phpMyAdmin.
- **Networks**: Connected to the `wordpress` network.

## Networks

- **wordpress**: A custom network for WordPress, MariaDB, and phpMyAdmin services.

## Volumes

- **db_data**: A named volume to persist MariaDB data.

## How to Use

1. Clone this repository:
   ```bash
   git clone https://github.com/semirhodzic/docker-wp.git
   ```
2. Navigate to the repository folder:
   ```bash
   cd docker-wp
   ```
3. Start the services using Docker Compose:
   ```bash
   docker-compose up -d
   ```
4. Access your WordPress site at `http://localhost`.
5. Access phpMyAdmin at `http://localhost:8080`.

## Stopping and Removing Containers

To stop and remove the containers, use:
```bash
docker-compose down
```

## Persistent Data

The database data is stored in the `db_data` volume, ensuring that your data persists even if the containers are stopped or removed.
