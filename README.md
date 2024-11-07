# Docker Laravel Configuration

This guide provides step-by-step instructions to set up a Laravel application using Docker with PostgreSQL and Nginx.

## Prerequisites

- **Docker** and **Docker Compose** installed on your machine.
- A **Laravel project** directory.

## Setup Instructions

Follow these steps to get your Laravel application up and running:

### 1. Copy Configuration Files

Copy all the files from this repository (except `README.md`) into your Laravel project directory.

```bash
# Navigate to your Laravel project directory
cd /path/to/your/laravel/project

# Copy the Docker configuration files
cp -r /path/to/docker-laravel-config/* ./
```

### 2. Create a `.env` File

If you don't already have a `.env` file in your Laravel project, create one:

```bash
cp .env.example .env
```

### 3. Configure Environment Variables

Edit your `.env` file to include the following database configuration:

```env
DB_CONNECTION=pgsql
DB_HOST=laravel-db
DB_PORT=5432
DB_DATABASE=docker_learn
DB_USERNAME=laravel
DB_PASSWORD=password
```

- **Note**: You can customize `DB_DATABASE`, `DB_USERNAME`, and `DB_PASSWORD` with your preferred values.

### 4. Build the Docker Image for the App Service

Build the Docker image for the `app` service defined in your `docker-compose.yml`:

```bash
docker-compose build app
```

### 5. Start the Docker Containers

Start all the services defined in your `docker-compose.yml` in detached mode:

```bash
docker-compose up -d
```

### 6. Install Composer Dependencies

Install the necessary PHP dependencies inside the `app` container:

```bash
docker-compose exec app composer install
```

### 7. Generate Application Key

Generate a new application key for your Laravel application:

```bash
docker-compose exec app php artisan key:generate
```

### 8. Run Database Migrations

Run the database migrations to set up your database schema:

```bash
docker-compose exec app php artisan migrate
```

### 9. Access Your Application

Your Laravel application should now be accessible at:

- **URL**: [http://localhost:8000](http://localhost:8000)

### 10. Optional: Seed the Database

If you have database seeders, you can run them:

```bash
docker-compose exec app php artisan db:seed
```

## Additional Commands

### Enter the App Container Shell

If you need to execute additional commands inside the `app` container:

```bash
docker-compose exec app sh
```

### Stop and Remove Containers

To stop all running containers and remove them along with their networks:

```bash
docker-compose down
```

### View Container Logs

To view real-time logs from your containers:

```bash
docker-compose logs -f
```

## File Structure Overview

Your Laravel project directory should now contain the following Docker-related files and directories:

- `docker-compose.yml`: Defines the Docker services.
- `Dockerfile`: Contains instructions to build the `app` service image.
- `docker-compose/`: Contains configuration files for Nginx and PostgreSQL initialization scripts.
  - `nginx/`: Nginx configuration files.
  - `postgresql/`: PostgreSQL initialization scripts (if any).

## Notes

- **Database Service**:
  - The database service uses the `bitnami/postgresql:16.3.0` image.
  - Ensure that any initialization scripts in `./docker-compose/postgresql` are compatible with PostgreSQL.

- **Environment Variables**:
  - Ensure that the database credentials in your `.env` file match the ones defined in the `docker-compose.yml` file.

- **File Permissions**:
  - You may need to adjust file permissions to allow the Docker containers to access your project files.

- **Network Configuration**:
  - All services are connected through a Docker network named `laravel`.

## Troubleshooting

- **Container Errors**:
  - Check the logs of a specific container:

    ```bash
    docker-compose logs <service_name>
    ```

    Replace `<service_name>` with `app`, `db`, or `nginx`.

- **Database Connection Issues**:
  - Ensure that the `DB_HOST` in your `.env` file matches the `container_name` or service name of your database in `docker-compose.yml` (`laravel-db`).

- **Port Conflicts**:
  - If port `8000` is already in use, change the port mapping in the `nginx` service in `docker-compose.yml`:

    ```yaml
    ports:
      - <desired_port>:80
    ```

## Conclusion

You have successfully set up a Laravel application using Docker with PostgreSQL and Nginx. You can now develop and run your application in a consistent environment across different systems.

---

Feel free to customize and extend this setup according to your project's needs.
