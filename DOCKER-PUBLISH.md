# Docker Publishing Guide

This guide explains how to publish the MechanicShop application using Docker for production deployment.

## Prerequisites

- Docker Desktop installed and running
- Access to the production database: `db20309.public.databaseasp.net`
- Production URL: `http://memorization.runasp.net`

## Production Docker Files

- **Dockerfile.production**: Production-optimized Dockerfile with Production environment settings
- **docker-compose.production.yml**: Production Docker Compose configuration
- **build-docker-production.ps1**: PowerShell script for Windows
- **build-docker-production.sh**: Bash script for Linux/Mac

## Quick Start

### Option 1: Using Docker Compose (Recommended)

```bash
# Build and start the production container
docker-compose -f docker-compose.production.yml up -d --build

# View logs
docker-compose -f docker-compose.production.yml logs -f

# Stop the container
docker-compose -f docker-compose.production.yml down
```

### Option 2: Using Docker Build Scripts

**Windows (PowerShell):**
```powershell
.\build-docker-production.ps1
```

**Linux/Mac (Bash):**
```bash
chmod +x build-docker-production.sh
./build-docker-production.sh
```

### Option 3: Manual Docker Build

```bash
# Build the image
docker build -f Dockerfile.production -t mechanicshop-api:production .

# Run the container
docker run -d \
  -p 80:80 \
  --name mechanicshop-api-prod \
  --env ASPNETCORE_ENVIRONMENT=Production \
  --env ConnectionStrings__DefaultConnection="Server=db20309.public.databaseasp.net;Database=db20309;User Id=db20309;Password=s_2S4J@hw?3G;Encrypt=False;MultipleActiveResultSets=True;" \
  --env AppSettings__AllowedOrigins__0="http://memorization.runasp.net" \
  --env AppSettings__AllowedOrigins__1="https://memorization.runasp.net" \
  --env JwtSettings__Issuer="memorization.runasp.net" \
  --env JwtSettings__Audience="memorization.runasp.net" \
  mechanicshop-api:production
```

## Configuration

The production Docker setup uses:

1. **Environment Variables** (set in docker-compose.production.yml):
   - `ASPNETCORE_ENVIRONMENT=Production`
   - Connection string to production database
   - CORS allowed origins
   - JWT issuer/audience settings

2. **appsettings.Production.json**: Automatically loaded when `ASPNETCORE_ENVIRONMENT=Production`

3. **web.config**: Included for IIS compatibility (if needed)

## Database Migration

Before running the container, ensure the database is migrated:

```bash
# Option 1: Run migrations from your local machine
cd src/MechanicShop.Api
dotnet ef database update --connection "Server=db20309.public.databaseasp.net;Database=db20309;User Id=db20309;Password=s_2S4J@hw?3G;Encrypt=False;MultipleActiveResultSets=True;"

# Option 2: Run migrations inside the container (if EF tools are available)
docker exec -it mechanicshop-api-prod dotnet ef database update
```

## Health Check

The container includes a health check endpoint. Monitor it with:

```bash
# Check container health
docker ps

# View health check logs
docker inspect mechanicshop-api-prod | grep -A 10 Health
```

## Troubleshooting

### View Logs
```bash
# Docker Compose
docker-compose -f docker-compose.production.yml logs -f

# Docker Run
docker logs -f mechanicshop-api-prod
```

### Container Not Starting
```bash
# Check container status
docker ps -a

# View detailed logs
docker logs mechanicshop-api-prod

# Inspect container
docker inspect mechanicshop-api-prod
```

### Database Connection Issues
- Verify the connection string is correct
- Check if the database server allows connections from your Docker host
- Ensure firewall rules allow connections to `db20309.public.databaseasp.net`

### Port Conflicts
If port 80 is already in use, change the port mapping:
```yaml
ports:
  - "8080:80"  # Maps host port 8080 to container port 80
```

## Publishing to Docker Registry

To publish to a Docker registry (e.g., Docker Hub, Azure Container Registry):

```bash
# Tag the image
docker tag mechanicshop-api:production your-registry/mechanicshop-api:production

# Push to registry
docker push your-registry/mechanicshop-api:production
```

## Differences from Development Docker

- **Environment**: Set to `Production` instead of `Development`
- **Database**: Uses production database connection string
- **CORS**: Configured for production domain
- **JWT**: Uses production domain for issuer/audience
- **Logging**: Simplified (no Seq integration)
- **Ports**: Exposes port 80 (standard HTTP port)

## Security Notes

- Never commit production credentials to version control
- Use Docker secrets or environment variables for sensitive data
- Consider using a secrets management service in production
- Regularly update base images for security patches

