# Docker Compose with GitHub Actions

A minimal project demonstrating how to use Docker Compose with GitHub Actions for CI/CD pipelines.

## Project Structure

```
.
├── docker-compose.yml          # Docker Compose configuration
└── .github/
    └── workflows/
        └── CI.yml              # GitHub Actions workflow
```

## Services

This project includes three Docker services using standard images:

### 🌐 Web Service (Nginx)
- **Image**: `nginx:alpine`
- **Port**: `8080`
- **Purpose**: Serves default nginx welcome page
- **Access**: http://localhost:8080

### 📱 App Service (Node.js)
- **Image**: `node:18-alpine`
- **Port**: `3000`
- **Purpose**: Simple Express.js server created at runtime
- **Access**: http://localhost:3000

### 🗄️ Database Service (PostgreSQL)
- **Image**: `postgres:15-alpine`
- **Port**: `5432`
- **Database**: `testdb`
- **User**: `testuser`
- **Password**: `testpass`

## Quick Start

### Prerequisites
- Docker
- Docker Compose

### Local Development

1. Start all services:
   ```bash
   docker-compose up -d
   ```

2. Check service status:
   ```bash
   docker-compose ps
   ```

3. Access the services:
   - Web (Nginx): http://localhost:8080
   - API (Node.js): http://localhost:3000
   - Database: localhost:5432

4. Stop all services:
   ```bash
   docker-compose down
   ```

### Viewing Logs

```bash
# All services
docker-compose logs

# Specific service
docker-compose logs web
docker-compose logs app
docker-compose logs db

# Follow logs in real-time
docker-compose logs -f
```

## GitHub Actions Workflow

The project includes a GitHub Actions workflow (`.github/workflows/CI.yml`) that:

### Triggers
- Push to `main` or `develop` branches
- Pull requests to `main` branch

### Workflow Steps

1. **Checkout Code**: Uses `actions/checkout@v4`
2. **Setup Docker Buildx**: Prepares Docker build environment
3. **Build & Run**: Executes `docker-compose up -d`
4. **Wait**: Allows services time to start up
5. **Health Checks**: Verifies all services are responding
6. **Basic Tests**: Tests connectivity to each service
7. **Log Collection**: Captures service logs for debugging
8. **Cleanup**: Removes containers and cleans up resources

### Service Health Checks

The workflow performs the following health checks:

- **Web Service**: HTTP request to port 8080 (nginx welcome page)
- **App Service**: HTTP request to port 3000 (JSON response)
- **Database**: PostgreSQL readiness check with `pg_isready`

## Development Tips

### Database Access

Connect to PostgreSQL directly:
```bash
docker-compose exec db psql -U testuser -d testdb
```

### Service Debugging

Execute commands in running containers:
```bash
# Access app container shell
docker-compose exec app sh

# Access database container
docker-compose exec db bash

# Check specific service logs
docker-compose logs app
```

### Troubleshooting

#### Common Issues

1. **Port conflicts**: Change ports in `docker-compose.yml` if they're already in use
2. **Services not starting**: Check logs with `docker-compose logs`
3. **Database connection**: Ensure the database service is fully started

#### Useful Commands

```bash
# Rebuild and restart services
docker-compose up --force-recreate

# Remove volumes (clean database)
docker-compose down -v

# Check resource usage
docker stats
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with `docker-compose up`
5. Submit a pull request

The GitHub Actions workflow will automatically test your changes!

## License

This project is open source and available under the [MIT License](LICENSE).