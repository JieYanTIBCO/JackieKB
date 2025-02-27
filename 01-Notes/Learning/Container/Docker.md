---
tags:
  - docker
  - cheatsheet
  - container
created: 2025-02-23
---

# Docker Cheat Sheet

## 🚀 Basic Commands

### Images

| Command | Description |
|---------|-------------|
| `docker pull <image:tag>` | Pull image from registry |
| `docker images` | List all images |
| `docker rmi <image>` | Remove image |
| `docker build -t <name:tag> .` | Build image from Dockerfile |
| `docker push <image>` | Push image to registry |
| `docker save -o <file.tar> <image>` | Export image to tar |
| `docker load -i <file.tar>` | Import image from tar |

### Containers

| Command                                  | Description                          |
| ---------------------------------------- | ------------------------------------ |
| `docker run -d --name <name> <image>`    | Run container in detached mode       |
| `docker run -it <image> /bin/bash`       | Run container with interactive shell |
| `docker run -p <host:container> <image>` | Run with port mapping                |
| `docker run -v <host:container> <image>` | Run with volume mount                |
| `docker ps`                              | List running containers              |
| `docker ps -a`                           | List all containers                  |
| `docker start/stop <container>`          | Start/stop container                 |
| `docker rm <container>`                  | Remove container                     |
| `docker exec -it <container> /bin/bash`  | Enter running container              |

## 🔍 Troubleshooting

### Logs & Inspection

| Command | Description |
|---------|-------------|
| `docker logs <container>` | View container logs |
| `docker logs -f <container>` | Follow container logs |
| `docker inspect <container>` | Show container details |
| `docker stats` | Show live resource usage |
| `docker top <container>` | Show container processes |

### Network Debugging

| Command | Description |
|---------|-------------|
| `docker network ls` | List networks |
| `docker network inspect <network>` | Inspect network |
| `docker network create <network>` | Create network |
| `docker run --network <network> <image>` | Run on specific network |

## 💾 Data Management

### Volumes

| Command                                               | Description   |
| ----------------------------------------------------- | ------------- |
| `docker volume create <name>`                         | Create volume |
| `docker volume ls`                                    | List volumes  |
| `docker volume rm <name>`                             | Remove volume |
| `docker run -v <HostVolume>:<ContainerPath)> <image>` | Mount volume  |

### System Cleanup

| Command | Description |
|---------|-------------|
| `docker system prune` | Remove unused data |
| `docker image prune` | Remove unused images |
| `docker container prune` | Remove stopped containers |
| `docker volume prune` | Remove unused volumes |

## 🔄 Docker Compose

| Command | Description |
|---------|-------------|
| `docker compose up -d` | Start services |
| `docker compose down` | Stop services |
| `docker compose logs` | View service logs |
| `docker compose build` | Build/rebuild services |

## 🛡️ Best Practices

1. **Image Optimization**
   - Use multi-stage builds
   - Minimize layers
   - Use `.dockerignore`

2. **Security**
   - Run as non-root user
   - Use official base images
   - Regularly update images

3. **Resource Management**
   - Set memory/CPU limits
   - Use health checks
   - Monitor container stats

## 🔗 Common Use Cases

### 1. Web Server (Nginx)

```bash
# Run Nginx with port mapping
docker run -d \
  --name my-nginx \
  -p 80:80 \                    # Map host port 80 to container port 80
  -v ./html:/usr/share/nginx/html:ro \  # Mount local files as read-only
  nginx:latest
```

### 2. Database Server (MySQL)

```bash
# Run MySQL with environment variables
docker run -d \
  --name my-mysql \
  -e MYSQL_ROOT_PASSWORD=secret \  # Set root password
  -e MYSQL_DATABASE=myapp \        # Create initial database
  -e MYSQL_USER=myuser \          # Create additional user
  -e MYSQL_PASSWORD=mypass \      # Set user password
  -v mysql-data:/var/lib/mysql \  # Persist data
  -p 3306:3306 \                  # Expose MySQL port
  mysql:8
```

### 3. Full Stack Application

```bash
# Create network for container communication
docker network create myapp-net

# Run backend API
docker run -d \
  --name backend \
  --network myapp-net \          # Join custom network
  -e DB_HOST=my-mysql \          # Use container name as hostname
  -v ./app:/app \               # Mount application code
  -p 8080:8080 \                # Expose API port
  myapp-backend:latest

# Run frontend
docker run -d \
  --name frontend \
  --network myapp-net \
  -p 3000:3000 \
  myapp-frontend:latest
```

### 4. Development Environment

```bash
# Run with hot reload and debugging
docker run -d \
  --name dev-env \
  -v $(pwd):/workspace \        # Mount current directory
  -p 3000:3000 \               # App port
  -p 9229:9229 \               # Debug port
  -e NODE_ENV=development \     # Set environment
  node:18
```

### 5. Database Backup

```bash
# Backup MySQL database to host
docker exec my-mysql \
  mysqldump -u root -p"secret" myapp > backup.sql

# Restore from backup
docker exec -i my-mysql \
  mysql -u root -p"secret" myapp < backup.sql
```

Each example includes:

- Container naming for easy reference
- Environment configuration
- Volume mounting for persistence
- Port mapping for access
- Network setup for container communication
- Common development scenarios
