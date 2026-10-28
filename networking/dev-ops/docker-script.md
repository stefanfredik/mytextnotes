---
icon: square-terminal
---

# Docker Script

## Docker Script Lengkap & Detail per Kategori

### 📦 KATEGORI 1: INSTALASI DOCKER

#### 1.1 Install Docker di Ubuntu/Debian

```bash
#!/bin/bash
# install-docker-ubuntu.sh

# Update package index
sudo apt-get update

# Install dependencies
sudo apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

# Add Docker's official GPG key
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Set up repository
echo \
  "deb [arch=$(dpkg --print-architecture) \
  signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt-get update
sudo apt-get install -y \
    docker-ce \
    docker-ce-cli \
    containerd.io \
    docker-buildx-plugin \
    docker-compose-plugin

# Add user to docker group
sudo usermod -aG docker $USER

# Start & enable Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Verify installation
docker --version
docker compose version
```

#### 1.2 Install Docker di CentOS/RHEL

```bash
#!/bin/bash
# install-docker-centos.sh

# Remove old versions
sudo yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine

# Install required packages
sudo yum install -y yum-utils

# Add Docker repository
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# Install Docker Engine
sudo yum install -y \
    docker-ce \
    docker-ce-cli \
    containerd.io \
    docker-buildx-plugin \
    docker-compose-plugin

# Start & enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# Add user to docker group
sudo usermod -aG docker $USER

echo "Docker installed successfully!"
docker --version
```

#### 1.3 Uninstall Docker

```bash
#!/bin/bash
# uninstall-docker.sh

# Stop Docker service
sudo systemctl stop docker
sudo systemctl stop docker.socket
sudo systemctl stop containerd

# Remove Docker packages
sudo apt-get purge -y \
    docker-ce \
    docker-ce-cli \
    containerd.io \
    docker-buildx-plugin \
    docker-compose-plugin \
    docker-ce-rootless-extras

# Remove Docker data
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
sudo rm -rf /etc/docker
sudo rm /etc/apt/keyrings/docker.gpg
sudo rm /etc/apt/sources.list.d/docker.list

echo "Docker uninstalled successfully!"
```

***

### 🖼️ KATEGORI 2: DOCKER IMAGE

#### 2.1 Pull Image

```bash
#!/bin/bash
# docker-image-pull.sh

# Pull image terbaru
docker pull nginx
docker pull nginx:latest

# Pull image versi spesifik
docker pull nginx:1.25
docker pull ubuntu:22.04
docker pull node:18-alpine
docker pull python:3.11-slim
docker pull mysql:8.0
docker pull postgres:15
docker pull redis:7-alpine
docker pull mongo:6.0

# Pull image dari registry lain
docker pull gcr.io/google-containers/pause:3.9
docker pull ghcr.io/username/image:tag
docker pull registry.gitlab.com/username/project/image:tag

# Pull semua tag dari image
docker pull --all-tags nginx
```

#### 2.2 List & Inspect Image

```bash
#!/bin/bash
# docker-image-list.sh

# List semua image
docker images
docker image ls

# List dengan format custom
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}\t{{.CreatedAt}}"

# Filter image
docker images --filter "dangling=true"
docker images --filter "before=nginx"
docker images --filter "since=nginx"
docker images nginx

# Inspect image detail
docker image inspect nginx
docker image inspect nginx --format '{{.Config.Env}}'
docker image inspect nginx --format '{{json .Config}}'

# Image history
docker image history nginx
docker history nginx --no-trunc

# Image digest
docker images --digests
docker inspect --format='{{index .RepoDigests 0}}' nginx
```

#### 2.3 Build Image

```bash
#!/bin/bash
# docker-image-build.sh

# Build basic
docker build -t myapp:latest .
docker build -t myapp:1.0 .

# Build dengan Dockerfile spesifik
docker build -f Dockerfile.prod -t myapp:prod .
docker build -f ./docker/Dockerfile -t myapp:dev .

# Build dengan build args
docker build \
    --build-arg NODE_ENV=production \
    --build-arg APP_VERSION=1.0.0 \
    --build-arg API_URL=https://api.example.com \
    -t myapp:latest .

# Build dengan target stage (multi-stage)
docker build --target development -t myapp:dev .
docker build --target production -t myapp:prod .

# Build dengan no cache
docker build --no-cache -t myapp:latest .

# Build dengan network
docker build --network=host -t myapp:latest .

# Build dengan label
docker build \
    --label version="1.0" \
    --label maintainer="admin@example.com" \
    -t myapp:latest .

# Build dengan memory limit
docker build --memory=2g -t myapp:latest .

# Build dan push sekaligus
docker build -t myrepo/myapp:latest . && docker push myrepo/myapp:latest

# Build menggunakan BuildKit (lebih cepat)
DOCKER_BUILDKIT=1 docker build -t myapp:latest .

# Build multi-platform
docker buildx build \
    --platform linux/amd64,linux/arm64 \
    -t myapp:latest \
    --push .
```

#### 2.4 Tag & Push Image

```bash
#!/bin/bash
# docker-image-tag-push.sh

# Tag image
docker tag myapp:latest myrepo/myapp:latest
docker tag myapp:latest myrepo/myapp:1.0
docker tag myapp:latest registry.example.com/myapp:latest

# Login ke Docker Hub
docker login
docker login -u username -p password

# Login ke registry lain
docker login registry.example.com
docker login -u username -p password registry.example.com
docker login ghcr.io -u username -p $GITHUB_TOKEN
docker login registry.gitlab.com

# Push image
docker push myrepo/myapp:latest
docker push myrepo/myapp:1.0
docker push registry.example.com/myapp:latest

# Logout
docker logout
docker logout registry.example.com
```

#### 2.5 Remove Image

```bash
#!/bin/bash
# docker-image-remove.sh

# Remove satu image
docker rmi nginx
docker image rm nginx
docker rmi nginx:latest

# Remove multiple images
docker rmi nginx mysql redis

# Remove dengan force
docker rmi -f nginx

# Remove image by ID
docker rmi abc123def456

# Remove dangling images (untagged)
docker image prune

# Remove dangling images tanpa konfirmasi
docker image prune -f

# Remove semua unused images
docker image prune -a
docker image prune -a -f

# Remove images berdasarkan filter
docker image prune -a --filter "until=24h"
docker image prune -a --filter "label=stage=test"

# Remove semua images
docker rmi $(docker images -q)
docker rmi $(docker images -aq)
```

#### 2.6 Save & Load Image

```bash
#!/bin/bash
# docker-image-save-load.sh

# Save image ke file tar
docker save nginx > nginx.tar
docker save nginx | gzip > nginx.tar.gz
docker save -o myapp.tar myapp:latest
docker save -o images.tar nginx mysql redis

# Load image dari file tar
docker load < nginx.tar
docker load < nginx.tar.gz
docker load -i myapp.tar
docker load --input images.tar

# Export container sebagai image
docker export container_name > container.tar
docker export -o container.tar container_name

# Import dari tar
docker import container.tar myimage:latest
cat container.tar | docker import - myimage:latest
```

***

### 🚀 KATEGORI 3: DOCKER CONTAINER

#### 3.1 Run Container

```bash
#!/bin/bash
# docker-container-run.sh

# Run basic
docker run nginx
docker run hello-world

# Run dengan nama
docker run --name my-nginx nginx

# Run detached (background)
docker run -d nginx
docker run -d --name my-nginx nginx

# Run interaktif
docker run -it ubuntu bash
docker run -it ubuntu /bin/bash
docker run -it --rm ubuntu bash

# Run dengan port mapping
docker run -d -p 80:80 nginx
docker run -d -p 8080:80 nginx
docker run -d -p 127.0.0.1:8080:80 nginx
docker run -d -p 8080:80 -p 443:443 nginx

# Run dengan environment variables
docker run -d \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_USER=myuser \
    -e MYSQL_PASSWORD=mypass \
    mysql:8.0

# Run dengan env file
docker run -d --env-file .env myapp:latest

# Run dengan volume
docker run -d \
    -v /host/path:/container/path \
    nginx

docker run -d \
    -v $(pwd)/html:/usr/share/nginx/html \
    nginx

# Run dengan named volume
docker run -d \
    -v myvolume:/var/lib/mysql \
    mysql:8.0

# Run dengan resource limits
docker run -d \
    --memory="512m" \
    --memory-swap="1g" \
    --cpus="1.5" \
    --cpu-shares=512 \
    nginx

# Run dengan restart policy
docker run -d --restart=always nginx
docker run -d --restart=unless-stopped nginx
docker run -d --restart=on-failure:5 nginx
docker run -d --restart=no nginx

# Run dengan network
docker run -d --network mynetwork nginx
docker run -d --network host nginx
docker run -d --network none nginx

# Run dengan hostname & DNS
docker run -d \
    --hostname myserver \
    --dns 8.8.8.8 \
    --dns 8.8.4.4 \
    nginx

# Run dengan user
docker run -d --user 1000:1000 nginx
docker run -d --user nobody nginx

# Run dengan working directory
docker run -it -w /app ubuntu bash

# Run dan hapus otomatis setelah selesai
docker run --rm ubuntu echo "Hello World"

# Run dengan log driver
docker run -d \
    --log-driver json-file \
    --log-opt max-size=10m \
    --log-opt max-file=3 \
    nginx

# Run dengan health check
docker run -d \
    --health-cmd="curl -f http://localhost/ || exit 1" \
    --health-interval=30s \
    --health-timeout=10s \
    --health-retries=3 \
    nginx

# Run dengan privileged mode
docker run -d --privileged nginx

# Run dengan cap-add/cap-drop
docker run -d \
    --cap-add NET_ADMIN \
    --cap-add SYS_PTRACE \
    ubuntu

# Run dengan SHM size
docker run -d --shm-size="256m" nginx

# Run dengan tmpfs
docker run -d \
    --tmpfs /tmp:rw,size=100m \
    nginx

# Run dengan device
docker run -d \
    --device /dev/sda:/dev/sda \
    ubuntu
```

#### 3.2 Manage Container

```bash
#!/bin/bash
# docker-container-manage.sh

# List container
docker ps                          # running containers
docker ps -a                       # semua containers
docker ps -q                       # hanya IDs
docker ps -aq                      # semua IDs
docker ps -l                       # container terakhir
docker ps -n 5                     # 5 container terakhir
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
docker ps --filter "status=running"
docker ps --filter "status=exited"
docker ps --filter "name=nginx"
docker ps --filter "ancestor=nginx"

# Start container
docker start container_name
docker start container_id
docker start container1 container2 container3

# Stop container
docker stop container_name
docker stop -t 30 container_name    # timeout 30 detik
docker stop $(docker ps -q)        # stop semua

# Restart container
docker restart container_name
docker restart -t 10 container_name

# Pause & Unpause
docker pause container_name
docker unpause container_name

# Kill container
docker kill container_name
docker kill --signal=SIGTERM container_name
docker kill $(docker ps -q)

# Remove container
docker rm container_name
docker rm -f container_name        # force remove (running)
docker rm -v container_name        # remove + volumes
docker rm $(docker ps -aq)         # remove semua
docker rm $(docker ps -aq -f status=exited)  # remove yang exited

# Container prune
docker container prune             # remove semua stopped containers
docker container prune -f          # tanpa konfirmasi

# Rename container
docker rename old_name new_name

# Update container resources
docker update --memory="1g" container_name
docker update --cpus="2" container_name
docker update --restart=always container_name
docker update --memory="1g" --cpus="2" container1 container2
```

#### 3.3 Inspect & Monitor Container

```bash
#!/bin/bash
# docker-container-inspect.sh

# Inspect container
docker inspect container_name
docker inspect container_name --format '{{.State.Status}}'
docker inspect container_name --format '{{.NetworkSettings.IPAddress}}'
docker inspect container_name --format '{{json .Config.Env}}'
docker inspect container_name --format '{{.HostConfig.PortBindings}}'

# Container logs
docker logs container_name
docker logs -f container_name          # follow/stream
docker logs --tail=100 container_name  # 100 baris terakhir
docker logs --since=1h container_name  # sejak 1 jam lalu
docker logs --since="2024-01-01" container_name
docker logs --until="2024-12-31" container_name
docker logs -t container_name          # dengan timestamp
docker logs --details container_name   # detail logs

# Container stats
docker stats                          # semua containers
docker stats container_name           # spesifik container
docker stats --no-stream              # snapshot sekali
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"

# Container processes
docker top container_name
docker top container_name aux

# Container resource usage
docker container stats --no-stream --format \
    "{{.Name}}: CPU={{.CPUPerc}} MEM={{.MemUsage}}"

# Diff filesystem container
docker diff container_name

# Container port mapping
docker port container_name
docker port container_name 80
```

#### 3.4 Execute Commands in Container

```bash
#!/bin/bash
# docker-container-exec.sh

# Execute command
docker exec container_name ls -la
docker exec container_name cat /etc/nginx/nginx.conf
docker exec container_name env

# Execute interaktif
docker exec -it container_name bash
docker exec -it container_name sh
docker exec -it container_name /bin/bash

# Execute dengan user
docker exec -it -u root container_name bash
docker exec -it -u 1000 container_name sh

# Execute dengan environment
docker exec -e MY_VAR=value container_name env
docker exec -it -e DEBUG=true container_name bash

# Execute dengan working directory
docker exec -it -w /app container_name bash

# Attach ke container
docker attach container_name
docker attach --detach-keys="ctrl-d" container_name
```

#### 3.5 Copy Files Container

```bash
#!/bin/bash
# docker-container-copy.sh

# Copy dari host ke container
docker cp ./local-file.txt container_name:/app/file.txt
docker cp ./config/ container_name:/etc/myapp/
docker cp ./html/. container_name:/usr/share/nginx/html/

# Copy dari container ke host
docker cp container_name:/app/logs/ ./logs/
docker cp container_name:/etc/nginx/nginx.conf ./nginx.conf
docker cp container_name:/var/log/app.log ./app.log
```

***

### 🗄️ KATEGORI 4: DOCKER VOLUME

#### 4.1 Manage Volume

```bash
#!/bin/bash
# docker-volume-manage.sh

# Buat volume
docker volume create myvolume
docker volume create \
    --driver local \
    --opt type=none \
    --opt device=/host/path \
    --opt o=bind \
    myvolume

# List volumes
docker volume ls
docker volume ls --filter "dangling=true"
docker volume ls --format "table {{.Name}}\t{{.Driver}}\t{{.Scope}}"

# Inspect volume
docker volume inspect myvolume
docker volume inspect myvolume --format '{{.Mountpoint}}'

# Remove volume
docker volume rm myvolume
docker volume rm volume1 volume2

# Remove unused volumes
docker volume prune
docker volume prune -f
docker volume prune --filter "label=temp=true"

# Remove semua volumes
docker volume rm $(docker volume ls -q)
```

#### 4.2 Volume Mount Types

```bash
#!/bin/bash
# docker-volume-mounts.sh

# Named Volume
docker run -d \
    --name mysql \
    -v mysql_data:/var/lib/mysql \
    mysql:8.0

# Bind Mount
docker run -d \
    --name nginx \
    -v /host/html:/usr/share/nginx/html:ro \
    nginx

# Bind Mount dengan mount flag
docker run -d \
    --name nginx \
    --mount type=bind,source=/host/html,target=/usr/share/nginx/html,readonly \
    nginx

# Volume dengan mount flag
docker run -d \
    --name mysql \
    --mount type=volume,source=mysql_data,target=/var/lib/mysql \
    mysql:8.0

# tmpfs Mount (memory only)
docker run -d \
    --name myapp \
    --tmpfs /tmp \
    myapp:latest

docker run -d \
    --name myapp \
    --mount type=tmpfs,destination=/tmp,tmpfs-size=100m \
    myapp:latest

# NFS Volume
docker volume create \
    --driver local \
    --opt type=nfs \
    --opt o=addr=192.168.1.100,rw \
    --opt device=:/path/to/nfs \
    nfs_volume

# Read-only volume
docker run -d \
    -v myvolume:/app/data:ro \
    myapp:latest
```

***

### 🌐 KATEGORI 5: DOCKER NETWORK

#### 5.1 Manage Network

```bash
#!/bin/bash
# docker-network-manage.sh

# List networks
docker network ls
docker network ls --filter "driver=bridge"
docker network ls --format "table {{.Name}}\t{{.Driver}}\t{{.Scope}}"

# Create network
docker network create mynetwork
docker network create --driver bridge mynetwork

# Create dengan subnet
docker network create \
    --driver bridge \
    --subnet 172.20.0.0/16 \
    --ip-range 172.20.240.0/20 \
    --gateway 172.20.0.1 \
    mynetwork

# Create overlay network (Swarm)
docker network create \
    --driver overlay \
    --attachable \
    myoverlay

# Create macvlan network
docker network create \
    --driver macvlan \
    --subnet=192.168.1.0/24 \
    --gateway=192.168.1.1 \
    -o parent=eth0 \
    mymacvlan

# Inspect network
docker network inspect mynetwork
docker network inspect mynetwork --format '{{json .Containers}}'
docker network inspect bridge

# Remove network
docker network rm mynetwork
docker network rm network1 network2

# Remove unused networks
docker network prune
docker network prune -f

# Connect container ke network
docker network connect mynetwork container_name
docker network connect --ip 172.20.0.10 mynetwork container_name
docker network connect --alias myalias mynetwork container_name

# Disconnect container dari network
docker network disconnect mynetwork container_name
docker network disconnect -f mynetwork container_name
```

#### 5.2 Network Modes

```bash
#!/bin/bash
# docker-network-modes.sh

# Bridge network (default)
docker run -d --network bridge nginx

# Host network
docker run -d --network host nginx

# None network
docker run -d --network none nginx

# Custom network
docker network create mynet
docker run -d --network mynet nginx

# Multiple networks
docker run -d --network mynet1 nginx
docker network connect mynet2 container_name

# Container network (share namespace)
docker run -d --name web nginx
docker run -d --network container:web alpine
```

***

### 📋 KATEGORI 6: DOCKERFILE

#### 6.1 Dockerfile Dasar

```dockerfile
# Dockerfile.basic

# Base image
FROM ubuntu:22.04

# Metadata
LABEL maintainer="admin@example.com"
LABEL version="1.0"
LABEL description="My Application"

# Environment variables
ENV APP_HOME=/app
ENV NODE_ENV=production
ENV PORT=3000

# Working directory
WORKDIR $APP_HOME

# Copy files
COPY . .
COPY package.json package-lock.json ./
COPY --chown=node:node . .

# Add (bisa extract tar, download URL)
ADD https://example.com/file.tar.gz /tmp/

# Run commands
RUN apt-get update && \
    apt-get install -y \
        curl \
        wget \
        vim \
    && rm -rf /var/lib/apt/lists/*

# Create user
RUN groupadd -r appuser && \
    useradd -r -g appuser appuser

# Switch user
USER appuser

# Expose port
EXPOSE 3000
EXPOSE 3000/tcp
EXPOSE 3000/udp

# Volume
VOLUME ["/app/data", "/app/logs"]

# Healthcheck
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:3000/health || exit 1

# Entrypoint
ENTRYPOINT ["node", "server.js"]

# CMD (default arguments)
CMD ["--port", "3000"]
```

#### 6.2 Dockerfile Node.js

```dockerfile
# Dockerfile.nodejs

FROM node:18-alpine AS base

WORKDIR /app

# Install dependencies stage
FROM base AS deps
COPY package*.json ./
RUN npm ci --only=production && \
    npm cache clean --force

# Build stage
FROM base AS builder
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM base AS production

# Create non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nextjs -u 1001

# Copy built application
COPY --from=deps --chown=nextjs:nodejs /app/node_modules ./node_modules
COPY --from=builder --chown=nextjs:nodejs /app/dist ./dist
COPY --from=builder --chown=nextjs:nodejs /app/package.json ./

USER nextjs

EXPOSE 3000

ENV NODE_ENV=production \
    PORT=3000

HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
    CMD wget -qO- http://localhost:3000/health || exit 1

CMD ["node", "dist/server.js"]
```

#### 6.3 Dockerfile Python

```dockerfile
# Dockerfile.python

FROM python:3.11-slim AS base

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONFAULTHANDLER=1 \
    PIP_NO_CACHE_DIR=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1 \
    PIP_DEFAULT_TIMEOUT=100

WORKDIR /app

# Install system dependencies
FROM base AS system-deps
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        libpq-dev \
        curl \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
FROM system-deps AS python-deps
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Production stage
FROM python-deps AS production

# Create non-root user
RUN groupadd -r appuser && \
    useradd -r -g appuser -d /app -s /sbin/nologin appuser

COPY --chown=appuser:appuser . .

USER appuser

EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
    CMD curl -f http://localhost:8000/health || exit 1

CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--workers", "4", "app:app"]
```

#### 6.4 Dockerfile Go

```dockerfile
# Dockerfile.golang

# Build stage
FROM golang:1.21-alpine AS builder

WORKDIR /build

# Cache dependencies
COPY go.mod go.sum ./
RUN go mod download && go mod verify

# Build application
COPY . .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
    go build -ldflags="-w -s" -o /app/server ./cmd/server

# Final stage (minimal image)
FROM scratch

# Copy certificates
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

# Copy binary
COPY --from=builder /app/server /server

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
    CMD ["/server", "health"]

ENTRYPOINT ["/server"]
```

#### 6.5 Dockerfile Nginx

```dockerfile
# Dockerfile.nginx

FROM nginx:alpine AS base

# Remove default config
RUN rm -rf /etc/nginx/conf.d/default.conf

# Copy custom config
COPY nginx.conf /etc/nginx/nginx.conf
COPY conf.d/ /etc/nginx/conf.d/

# Build frontend (multi-stage)
FROM node:18-alpine AS frontend-builder
WORKDIR /app
COPY frontend/package*.json ./
RUN npm ci
COPY frontend/ .
RUN npm run build

# Final stage
FROM base AS production

# Copy built frontend
COPY --from=frontend-builder /app/dist /usr/share/nginx/html

# Set permissions
RUN chown -R nginx:nginx /usr/share/nginx/html && \
    chmod -R 755 /usr/share/nginx/html

EXPOSE 80 443

HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
    CMD wget -qO- http://localhost/health || exit 1

CMD ["nginx", "-g", "daemon off;"]
```

***

### 🐙 KATEGORI 7: DOCKER COMPOSE

#### 7.1 Docker Compose Dasar

```yaml
# docker-compose.yml

version: '3.9'

services:
  web:
    image: nginx:alpine
    container_name: web-server
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./html:/usr/share/nginx/html:ro
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    networks:
      - frontend
    depends_on:
      - app
    healthcheck:
      test: ["CMD", "wget", "-qO-", "http://localhost/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 10s

  app:
    build:
      context: .
      dockerfile: Dockerfile
      args:
        - NODE_ENV=production
      target: production
    container_name: app-server
    restart: unless-stopped
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DB_HOST=database
      - DB_PORT=5432
      - DB_NAME=${DB_NAME}
      - DB_USER=${DB_USER}
      - DB_PASSWORD=${DB_PASSWORD}
      - REDIS_URL=redis://cache:6379
    env_file:
      - .env
    volumes:
      - app_logs:/app/logs
      - app_uploads:/app/uploads
    networks:
      - frontend
      - backend
    depends_on:
      database:
        condition: service_healthy
      cache:
        condition: service_healthy

  database:
    image: postgres:15-alpine
    container_name: postgres-db
    restart: unless-stopped
    environment:
      - POSTGRES_DB=${DB_NAME}
      - POSTGRES_USER=${DB_USER}
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./sql/init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - backend
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 30s

  cache:
    image: redis:7-alpine
    container_name: redis-cache
    restart: unless-stopped
    command: redis-server --appendonly yes --requirepass ${REDIS_PASSWORD}
    volumes:
      - redis_data:/data
    networks:
      - backend
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 3

  adminer:
    image: adminer:latest
    container_name: db-adminer
    restart: unless-stopped
    ports:
      - "8080:8080"
    networks:
      - backend
    profiles:
      - debug

volumes:
  postgres_data:
    driver: local
  redis_data:
    driver: local
  app_logs:
    driver: local
  app_uploads:
    driver: local

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
    internal: true
```

#### 7.2 Docker Compose Commands

```bash
#!/bin/bash
# docker-compose-commands.sh

# Start services
docker compose up
docker compose up -d                   # detached
docker compose up -d --build           # rebuild images
docker compose up web app              # spesifik service
docker compose up --scale app=3        # scale service
docker compose up --force-recreate     # recreate containers

# Stop services
docker compose down
docker compose down -v                 # remove volumes
docker compose down --rmi all         # remove images
docker compose down --remove-orphans  # remove orphan containers

# Build services
docker compose build
docker compose build --no-cache
docker compose build --parallel
docker compose build web app

# Start/Stop/Restart
docker compose start
docker compose stop
docker compose restart
docker compose restart web

# Logs
docker compose logs
docker compose logs -f
docker compose logs -f --tail=100
docker compose logs web app

# Execute commands
docker compose exec web bash
docker compose exec -it web bash
docker compose exec -u root web bash
docker compose exec web nginx -t
docker compose exec database psql -U user -d mydb

# Run one-off command
docker compose run web bash
docker compose run --rm web bash
docker compose run --rm app python manage.py migrate

# List containers
docker compose ps
docker compose ps -a

# List services
docker compose ls

# Pull images
docker compose pull
docker compose pull web

# Push images
docker compose push

# Show config
docker compose config
docker compose config --services
docker compose config --volumes

# Scale service
docker compose scale app=5
docker compose up -d --scale app=5

# Copy files
docker compose cp web:/app/logs ./logs

# Events
docker compose events
docker compose events web

# Top (processes)
docker compose top
docker compose top web

# Pause/Unpause
docker compose pause
docker compose unpause

# Kill
docker compose kill
docker compose kill -s SIGTERM
```

#### 7.3 Docker Compose LEMP Stack

```yaml
# docker-compose-lemp.yml

version: '3.9'

services:
  nginx:
    image: nginx:alpine
    container_name: lemp-nginx
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./src:/var/www/html
      - ./nginx/conf.d:/etc/nginx/conf.d
      - ./nginx/ssl:/etc/nginx/ssl
      - ./nginx/logs:/var/log/nginx
    networks:
      - lemp-network
    depends_on:
      - php

  php:
    build:
      context: ./php
      dockerfile: Dockerfile
    container_name: lemp-php
    restart: unless-stopped
    volumes:
      - ./src:/var/www/html
      - ./php/php.ini:/usr/local/etc/php/conf.d/custom.ini
    networks:
      - lemp-network
    depends_on:
      - mysql
    environment:
      - DB_HOST=mysql
      - DB_PORT=3306
      - DB_NAME=${MYSQL_DATABASE}
      - DB_USER=${MYSQL_USER}
      - DB_PASSWORD=${MYSQL_PASSWORD}

  mysql:
    image: mysql:8.0
    container_name: lemp-mysql
    restart: unless-stopped
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
    volumes:
      - mysql_data:/var/lib/mysql
      - ./mysql/init:/docker-entrypoint-initdb.d
      - ./mysql/conf.d:/etc/mysql/conf.d
    networks:
      - lemp-network
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 5s
      retries: 5

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: lemp-phpmyadmin
    restart: unless-stopped
    ports:
      - "8080:80"
    environment:
      - PMA_HOST=mysql
      - PMA_PORT=3306
      - UPLOAD_LIMIT=100M
    networks:
      - lemp-network
    depends_on:
      - mysql

volumes:
  mysql_data:

networks:
  lemp-network:
    driver: bridge
```

#### 7.4 Docker Compose Monitoring Stack

```yaml
# docker-compose-monitoring.yml

version: '3.9'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    restart: unless-stopped
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - ./prometheus/rules:/etc/prometheus/rules
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.console.libraries=/etc/prometheus/console_libraries'
      - '--web.console.templates=/etc/prometheus/consoles'
      - '--storage.tsdb.retention.time=30d'
      - '--web.enable-lifecycle'
      - '--web.enable-admin-api'
    networks:
      - monitoring

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    restart: unless-stopped
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=${GRAFANA_USER}
      - GF_SECURITY_ADMIN_PASSWORD=${GRAFANA_PASSWORD}
      - GF_USERS_ALLOW_SIGN_UP=false
      - GF_INSTALL_PLUGINS=grafana-clock-panel,grafana-simple-json-datasource
    volumes:
      - grafana_data:/var/lib/grafana
      - ./grafana/provisioning:/etc/grafana/provisioning
      - ./grafana/dashboards:/var/lib/grafana/dashboards
    networks:
      - monitoring
    depends_on:
      - prometheus

  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    restart: unless-stopped
    ports:
      - "9100:9100"
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    command:
      - '--path.procfs=/host/proc'
      - '--path.rootfs=/rootfs'
      - '--path.sysfs=/host/sys'
      - '--collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)'
    networks:
      - monitoring

  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    container_name: cadvisor
    restart: unless-stopped
    ports:
      - "8080:8080"
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
      - /dev/disk/:/dev/disk:ro
    privileged: true
    devices:
      - /dev/kmsg
    networks:
      - monitoring

  alertmanager:
    image: prom/alertmanager:latest
    container_name: alertmanager
    restart: unless-stopped
    ports:
      - "9093:9093"
    volumes:
      - ./alertmanager/alertmanager.yml:/etc/alertmanager/alertmanager.yml
      - alertmanager_data:/alertmanager
    networks:
      - monitoring

volumes:
  prometheus_data:
  grafana_data:
  alertmanager_data:

networks:
  monitoring:
    driver: bridge
```

***

### 🔧 KATEGORI 8: DOCKER REGISTRY

#### 8.1 Private Registry

```bash
#!/bin/bash
# docker-registry-setup.sh

# Run private registry
docker run -d \
    --name registry \
    --restart=always \
    -p 5000:5000 \
    -v registry_data:/var/lib/registry \
    registry:2

# Registry dengan authentication
mkdir -p /auth
docker run --rm \
    --entrypoint htpasswd \
    httpd:2 -Bbn admin password > /auth/htpasswd

docker run -d \
    --name registry \
    --restart=always \
    -p 5000:5000 \
    -v registry_data:/var/lib/registry \
    -v /auth:/auth \
    -e REGISTRY_AUTH=htpasswd \
    -e REGISTRY_AUTH_HTPASSWD_REALM="Registry Realm" \
    -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
    registry:2

# Registry dengan SSL
docker run -d \
    --name registry \
    --restart=always \
    -p 443:443 \
    -v registry_data:/var/lib/registry \
    -v /certs:/certs \
    -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
    -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
    -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
    registry:2

# Push ke private registry
docker tag myapp:latest localhost:5000/myapp:latest
docker push localhost:5000/myapp:latest

# Pull dari private registry
docker pull localhost:5000/myapp:latest

# List images di registry
curl http://localhost:5000/v2/_catalog
curl http://localhost:5000/v2/myapp/tags/list

# Delete image dari registry
curl -X DELETE http://localhost:5000/v2/myapp/manifests/<digest>
```

***

### 🔒 KATEGORI 9: DOCKER SECURITY

#### 9.1 Security Best Practices

```bash
#!/bin/bash
# docker-security.sh

# Scan image dengan Docker Scout
docker scout cves nginx:latest
docker scout recommendations nginx:latest
docker scout quickview nginx:latest

# Scan dengan Trivy
docker run --rm \
    -v /var/run/docker.sock:/var/run/docker.sock \
    aquasec/trivy:latest image nginx:latest

# Run sebagai non-root user
docker run --user 1000:1000 nginx

# Read-only filesystem
docker run --read-only nginx

# Drop capabilities
docker run \
    --cap-drop ALL \
    --cap-add NET_BIND_SERVICE \
    nginx

# Seccomp profile
docker run \
    --security-opt seccomp=/path/to/seccomp.json \
    nginx

# AppArmor profile
docker run \
    --security-opt apparmor=docker-default \
    nginx

# No new privileges
docker run \
    --security-opt no-new-privileges:true \
    nginx

# Memory limits (prevent DoS)
docker run \
    --memory="512m" \
    --memory-swap="512m" \
    --pids-limit 100 \
    nginx

# Limit syscalls
docker run \
    --security-opt seccomp=unconfined \
    nginx

# Inspect security options
docker inspect --format='{{.HostConfig.SecurityOpt}}' container_name

# Check running as root
docker inspect --format='{{.Config.User}}' container_name
```

#### 9.2 Docker Content Trust

```bash
#!/bin/bash
# docker-content-trust.sh

# Enable Docker Content Trust
export DOCKER_CONTENT_TRUST=1

# Generate keys
docker trust key generate mykey

# Sign image
docker trust sign myrepo/myimage:latest

# Inspect trust data
docker trust inspect myrepo/myimage

# View signers
docker trust inspect --pretty myrepo/myimage

# Remove trust data
docker trust remove myrepo/myimage

# Revoke trust
docker trust revoke myrepo/myimage:latest
```

***

### 📊 KATEGORI 10: DOCKER SWARM

#### 10.1 Setup Swarm

```bash
#!/bin/bash
# docker-swarm-setup.sh

# Initialize Swarm (pada manager node)
docker swarm init
docker swarm init --advertise-addr 192.168.1.100

# Get join tokens
docker swarm join-token worker
docker swarm join-token manager

# Join sebagai worker
docker swarm join \
    --token SWMTKN-1-xxxxx \
    192.168.1.100:2377

# Join sebagai manager
docker swarm join \
    --token SWMTKN-1-xxxxx \
    192.168.1.100:2377

# List nodes
docker node ls
docker node inspect node_name
docker node inspect --pretty node_name

# Promote worker ke manager
docker node promote worker_node

# Demote manager ke worker
docker node demote manager_node

# Remove node dari swarm
docker node rm node_name
docker node rm --force node_name

# Update node
docker node update --availability drain node_name
docker node update --availability active node_name
docker node update --label-add env=prod node_name

# Leave swarm
docker swarm leave
docker swarm leave --force        # untuk manager

# Unlock swarm (setelah restart)
docker swarm unlock

# Rotate unlock key
docker swarm unlock-key --rotate
```

#### 10.2 Swarm Services

```bash
#!/bin/bash
# docker-swarm-services.sh

# Create service
docker service create --name web nginx
docker service create \
    --name web \
    --replicas 3 \
    --publish published=80,target=80 \
    nginx

# Create service dengan constraints
docker service create \
    --name web \
    --replicas 3 \
    --constraint 'node.role==worker' \
    --constraint 'node.labels.env==prod' \
    nginx

# Create service dengan update config
docker service create \
    --name web \
    --replicas 3 \
    --update-parallelism 1 \
    --update-delay 10s \
    --update-failure-action rollback \
    nginx

# Create service dengan secrets
docker service create \
    --name web \
    --secret db_password \
    nginx

# List services
docker service ls
docker service ps web
docker service inspect web
docker service inspect --pretty web

# Update service
docker service update --image nginx:1.25 web
docker service update --replicas 5 web
docker service update --env-add NEW_VAR=value web
docker service update --publish-add 8080:80 web
docker service update --force web           # force redeploy

# Scale service
docker service scale web=5
docker service scale web=5 api=3

# Remove service
docker service rm web
docker service rm service1 service2

# Service logs
docker service logs web
docker service logs -f web
docker service logs --tail=100 web

# Rollback service
docker service rollback web
```

#### 10.3 Docker Stack

```bash
#!/bin/bash
# docker-swarm-stack.sh

# Deploy stack
docker stack deploy -c docker-compose.yml mystack
docker stack deploy -c docker-compose.yml -c docker-compose.prod.yml mystack

# List stacks
docker stack ls

# List services dalam stack
docker stack services mystack

# List tasks dalam stack
docker stack ps mystack
docker stack ps --no-trunc mystack

# Remove stack
docker stack rm mystack
```

***

### 🔐 KATEGORI 11: DOCKER SECRETS & CONFIGS

#### 11.1 Docker Secrets

```bash
#!/bin/bash
# docker-secrets.sh

# Create secret dari stdin
echo "mysecretpassword" | docker secret create db_password -

# Create secret dari file
docker secret create db_password ./password.txt
docker secret create ssl_cert ./cert.pem

# List secrets
docker secret ls

# Inspect secret
docker secret inspect db_password

# Remove secret
docker secret rm db_password

# Use secret dalam service
docker service create \
    --name myapp \
    --secret db_password \
    --secret source=ssl_cert,target=/run/secrets/cert.pem,mode=0400 \
    myapp:latest

# Use secret dalam compose (Swarm mode)
# docker-compose-secrets.yml:
# secrets:
#   db_password:
#     external: true
```

#### 11.2 Docker Configs

```bash
#!/bin/bash
# docker-configs.sh

# Create config
docker config create nginx_config ./nginx.conf
echo "server { listen 80; }" | docker config create nginx_config -

# List configs
docker config ls

# Inspect config
docker config inspect nginx_config
docker config inspect --pretty nginx_config

# Remove config
docker config rm nginx_config

# Use config dalam service
docker service create \
    --name nginx \
    --config source=nginx_config,target=/etc/nginx/nginx.conf,mode=0444 \
    nginx
```

***

### 🛠️ KATEGORI 12: DOCKER SYSTEM & MAINTENANCE

#### 12.1 System Commands

```bash
#!/bin/bash
# docker-system.sh

# System info
docker info
docker version
docker system info

# Disk usage
docker system df
docker system df -v

# System prune (cleanup everything)
docker system prune
docker system prune -f
docker system prune -a               # termasuk unused images
docker system prune -a --volumes     # termasuk volumes
docker system prune -a -f --volumes  # tanpa konfirmasi

# Events
docker system events
docker events
docker events --filter 'type=container'
docker events --filter 'event=start'
docker events --since '1h'
docker events --until '2024-12-31'

# Resource usage
docker stats --no-stream
docker stats --all --no-stream

# Context
docker context ls
docker context create mycontext --docker "host=tcp://remote:2376"
docker context use mycontext
docker context rm mycontext
```

#### 12.2 Cleanup Scripts

```bash
#!/bin/bash
# docker-cleanup.sh

echo "=== Docker Cleanup Script ==="

# Stop semua containers
echo "Stopping all containers..."
docker stop $(docker ps -q) 2>/dev/null || true

# Remove semua stopped containers
echo "Removing stopped containers..."
docker container prune -f

# Remove semua unused images
echo "Removing unused images..."
docker image prune -a -f

# Remove semua unused volumes
echo "Removing unused volumes..."
docker volume prune -f

# Remove semua unused networks
echo "Removing unused networks..."
docker network prune -f

# Full cleanup
echo "Running full system prune..."
docker system prune -a -f --volumes

echo "=== Cleanup Complete ==="
docker system df
```

#### 12.3 Backup & Restore

```bash
#!/bin/bash
# docker-backup-restore.sh

BACKUP_DIR="/backup/docker"
DATE=$(date +%Y%m%d_%H%M%S)

# Backup volume
backup_volume() {
    local volume_name=$1
    local backup_file="$BACKUP_DIR/${volume_name}_${DATE}.tar.gz"
    
    echo "Backing up volume: $volume_name"
    docker run --rm \
        -v $volume_name:/data \
        -v $BACKUP_DIR:/backup \
        alpine \
        tar czf /backup/${volume_name}_${DATE}.tar.gz -C /data .
    
    echo "Backup saved: $backup_file"
}

# Restore volume
restore_volume() {
    local volume_name=$1
    local backup_file=$2
    
    echo "Restoring volume: $volume_name from $backup_file"
    docker volume create $volume_name 2>/dev/null || true
    docker run --rm \
        -v $volume_name:/data \
        -v $(dirname $backup_file):/backup \
        alpine \
        tar xzf /backup/$(basename $backup_file) -C /data
    
    echo "Volume restored successfully"
}

# Backup all volumes
backup_all_volumes() {
    mkdir -p $BACKUP_DIR
    for volume in $(docker volume ls -q); do
        backup_volume $volume
    done
}

# Backup container
backup_container() {
    local container_name=$1
    echo "Backing up container: $container_name"
    docker commit $container_name ${container_name}_backup:${DATE}
    docker save ${container_name}_backup:${DATE} | \
        gzip > $BACKUP_DIR/${container_name}_${DATE}.tar.gz
    echo "Container backup saved"
}

# Usage
backup_all_volumes
# backup_volume "mysql_data"
# restore_volume "mysql_data" "/backup/docker/mysql_data_20240101_120000.tar.gz"
```

***

### 🔄 KATEGORI 13: DOCKER CI/CD

#### 13.1 Build & Push Script

```bash
#!/bin/bash
# docker-cicd-build.sh

# Variables
REGISTRY="registry.example.com"
IMAGE_NAME="myapp"
BRANCH=$(git rev-parse --abbrev-ref HEAD)
COMMIT=$(git rev-parse --short HEAD)
TAG="${BRANCH}-${COMMIT}"
DATE=$(date +%Y%m%d)

# Build image
docker build \
    --build-arg BUILD_DATE=$DATE \
    --build-arg VCS_REF=$COMMIT \
    --build-arg VERSION=$TAG \
    --label "org.label-schema.build-date=$DATE" \
    --label "org.label-schema.vcs-ref=$COMMIT" \
    --label "org.label-schema.version=$TAG" \
    -t $REGISTRY/$IMAGE_NAME:$TAG \
    -t $REGISTRY/$IMAGE_NAME:latest \
    .

# Test image
docker run --rm \
    $REGISTRY/$IMAGE_NAME:$TAG \
    npm test

# Push jika test berhasil
if [ $? -eq 0 ]; then
    docker push $REGISTRY/$IMAGE_NAME:$TAG
    docker push $REGISTRY/$IMAGE_NAME:latest
    echo "Successfully pushed $REGISTRY/$IMAGE_NAME:$TAG"
else
    echo "Tests failed, not pushing image"
    exit 1
fi
```

#### 13.2 Deploy Script

```bash
#!/bin/bash
# docker-deploy.sh

set -e

REGISTRY="registry.example.com"
IMAGE="myapp"
TAG=${1:-latest}
ENV=${2:-production}

echo "Deploying $IMAGE:$TAG to $ENV..."

# Pull terbaru
docker pull $REGISTRY/$IMAGE:$TAG

# Blue-Green deployment
CURRENT=$(docker ps --filter "name=app-blue" --format "{{.Names}}" | head -1)

if [ "$CURRENT" = "app-blue" ]; then
    NEW_NAME="app-green"
    OLD_NAME="app-blue"
else
    NEW_NAME="app-blue"
    OLD_NAME="app-green"
fi

# Start new container
docker run -d \
    --name $NEW_NAME \
    --network mynetwork \
    -e ENV=$ENV \
    $REGISTRY/$IMAGE:$TAG

# Health check
echo "Waiting for health check..."
sleep 10

HEALTH=$(docker inspect --format='{{.State.Health.Status}}' $NEW_NAME)

if [ "$HEALTH" = "healthy" ]; then
    echo "$NEW_NAME is healthy"
    
    # Update load balancer (nginx reload)
    # ...
    
    # Stop old container
    if docker ps -q --filter "name=$OLD_NAME" | grep -q .; then
        docker stop $OLD_NAME
        docker rm $OLD_NAME
        echo "Stopped and removed $OLD_NAME"
    fi
    
    echo "Deployment successful!"
else
    echo "Health check failed! Rolling back..."
    docker stop $NEW_NAME
    docker rm $NEW_NAME
    exit 1
fi
```

***

### 📈 KATEGORI 14: DOCKER LOGGING

#### 14.1 Log Configuration

```bash
#!/bin/bash
# docker-logging.sh

# JSON File logging (default)
docker run -d \
    --log-driver json-file \
    --log-opt max-size=10m \
    --log-opt max-file=3 \
    --log-opt compress=true \
    nginx

# Syslog logging
docker run -d \
    --log-driver syslog \
    --log-opt syslog-address=udp://192.168.1.1:514 \
    --log-opt syslog-facility=daemon \
    --log-opt tag="myapp" \
    nginx

# Journald logging
docker run -d \
    --log-driver journald \
    --log-opt tag="myapp" \
    nginx

# Gelf logging (Graylog)
docker run -d \
    --log-driver gelf \
    --log-opt gelf-address=udp://graylog:12201 \
    --log-opt tag="myapp" \
    nginx

# Fluentd logging
docker run -d \
    --log-driver fluentd \
    --log-opt fluentd-address=localhost:24224 \
    --log-opt tag="myapp.{{.Name}}" \
    nginx

# AWS CloudWatch
docker run -d \
    --log-driver awslogs \
    --log-opt awslogs-region=us-east-1 \
    --log-opt awslogs-group=myapp \
    --log-opt awslogs-stream=myapp-stream \
    nginx

# None (disable logging)
docker run -d \
    --log-driver none \
    nginx

# Global log config di /etc/docker/daemon.json
cat > /etc/docker/daemon.json << 'EOF'
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3",
    "compress": "true"
  }
}
EOF
```

***

### ⚙️ KATEGORI 15: DOCKER DAEMON CONFIGURATION

#### 15.1 daemon.json Configuration

```json
// /etc/docker/daemon.json

{
  "debug": false,
  "log-level": "info",
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m",
    "max-file": "5"
  },
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ],
  "data-root": "/var/lib/docker",
  "exec-root": "/var/run/docker",
  "dns": ["8.8.8.8", "8.8.4.4"],
  "dns-search": ["example.com"],
  "default-ulimits": {
    "nofile": {
      "Name": "nofile",
      "Hard": 64000,
      "Soft": 64000
    }
  },
  "live-restore": true,
  "userland-proxy": false,
  "no-new-privileges": true,
  "default-runtime": "runc",
  "runtimes": {
    "nvidia": {
      "path": "nvidia-container-runtime",
      "runtimeArgs": []
    }
  },
  "insecure-registries": [],
  "registry-mirrors": [
    "https://mirror.example.com"
  ],
  "max-concurrent-downloads": 10,
  "max-concurrent-uploads": 5,
  "experimental": false,
  "metrics-addr": "127.0.0.1:9323",
  "ipv6": false,
  "fixed-cidr-ipv6": "",
  "bip": "172.17.0.1/16",
  "default-address-pools": [
    {
      "base": "172.30.0.0/16",
      "size": 24
    }
  ]
}
```

#### 15.2 Daemon Management

```bash
#!/bin/bash
# docker-daemon-manage.sh

# Reload daemon config
sudo systemctl daemon-reload
sudo systemctl restart docker

# Check daemon status
sudo systemctl status docker
sudo journalctl -u docker -f

# Docker daemon info
docker info
docker system info

# Check docker socket
ls -la /var/run/docker.sock

# Remote Docker daemon
export DOCKER_HOST=tcp://remote-host:2376
export DOCKER_TLS_VERIFY=1
export DOCKER_CERT_PATH=~/.docker/certs

# Atau dengan context
docker context create remote \
    --docker "host=tcp://remote:2376,ca=~/.docker/ca.pem,cert=~/.docker/cert.pem,key=~/.docker/key.pem"
docker context use remote
```

***

### 🔍 KATEGORI 16: DOCKER TROUBLESHOOTING

#### 16.1 Troubleshooting Commands

```bash
#!/bin/bash
# docker-troubleshoot.sh

# Check container status
docker ps -a
docker inspect container_name
docker stats --no-stream

# Container tidak bisa start
docker logs container_name
docker events --filter container=container_name

# Check image
docker history image_name
docker inspect image_name

# Network issues
docker network inspect bridge
docker exec container_name ping google.com
docker exec container_name nslookup google.com
docker exec container_name curl -v http://other-container

# Volume issues
docker inspect container_name | grep -A 20 Mounts
docker volume inspect volume_name

# Resource issues
docker stats
docker system df
df -h
free -h

# Container OOM (Out of Memory)
docker inspect container_name | grep -i oom
dmesg | grep -i "out of memory"

# Docker daemon issues
sudo journalctl -u docker --since "1 hour ago"
sudo journalctl -u docker -n 100

# Check Docker events
docker events --since '30m'

# Container network debugging
docker run --rm \
    --network container:container_name \
    nicolaka/netshoot \
    tcpdump -i eth0

# Inspect all containers health
docker ps --format "{{.Names}}: {{.Status}}" | grep -v healthy

# Find containers using most resources
docker stats --no-stream --format \
    "{{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}" | sort -t$'\t' -k2 -rn | head -10

# Check container filesystem
docker diff container_name
docker export container_name | tar tv | head -50

# Debug dengan strace
docker run --rm \
    --pid=container:container_name \
    --cap-add SYS_PTRACE \
    ubuntu \
    strace -p 1
```

#### 16.2 Health Check Script

```bash
#!/bin/bash
# docker-health-check.sh

echo "=== Docker Health Check ==="
echo ""

# Docker version
echo "Docker Version:"
docker version --format 'Client: {{.Client.Version}}, Server: {{.Server.Engine.Version}}'
echo ""

# Running containers
echo "Running Containers: $(docker ps -q | wc -l)"
echo "Total Containers: $(docker ps -aq | wc -l)"
echo ""

# Images
echo "Total Images: $(docker images -q | wc -l)"
echo ""

# Volumes
echo "Total Volumes: $(docker volume ls -q | wc -l)"
echo "Unused Volumes: $(docker volume ls -qf dangling=true | wc -l)"
echo ""

# Disk usage
echo "Docker Disk Usage:"
docker system df
echo ""

# Unhealthy containers
echo "Unhealthy Containers:"
docker ps --filter "health=unhealthy" --format "  - {{.Names}} ({{.Status}})"
echo ""

# Containers using high CPU
echo "High CPU Containers (>50%):"
docker stats --no-stream --format "{{.Name}}: {{.CPUPerc}}" | \
    awk -F: '{gsub(/%/,"",$2); if($2>50) print "  - "$0}'
echo ""

echo "=== Health Check Complete ==="
```

***

### 📝 KATEGORI 17: .DOCKERIGNORE

```plaintext
# .dockerignore

# Git
.git
.gitignore
.gitattributes

# Docker
Dockerfile
Dockerfile.*
docker-compose.yml
docker-compose.*.yml
.dockerignore

# Node.js
node_modules
npm-debug.log
yarn-error.log
.npm
.yarn

# Python
__pycache__
*.pyc
*.pyo
*.pyd
.Python
env
venv
.venv
pip-log.txt
.pytest_cache
*.egg-info
dist
build

# OS
.DS_Store
Thumbs.db
*.swp
*.swo

# IDE
.vscode
.idea
*.iml
.eclipse
.project
.classpath

# Logs
logs
*.log
log

# Tests
test
tests
coverage
.coverage
*.test.js

# Documentation
docs
README.md
CHANGELOG.md
*.md

# CI/CD
.github
.gitlab-ci.yml
.travis.yml
Jenkinsfile

# Secrets (PENTING!)
.env
.env.*
*.pem
*.key
*.cert
secrets/
credentials/
```

***

### 📋 RINGKASAN COMMANDS

```bash
#!/bin/bash
# docker-quick-reference.sh

# ============ IMAGE ============
docker pull IMAGE              # Download image
docker push IMAGE              # Upload image
docker build -t NAME .         # Build image
docker images                  # List images
docker rmi IMAGE               # Remove image
docker image prune -a          # Remove unused images
docker save IMAGE > file.tar   # Save image
docker load < file.tar         # Load image

# ============ CONTAINER ============
docker run IMAGE               # Run container
docker run -d IMAGE            # Run detached
docker run -it IMAGE bash      # Run interactive
docker ps                      # List running
docker ps -a                   # List all
docker start NAME              # Start container
docker stop NAME               # Stop container
docker restart NAME            # Restart container
docker rm NAME                 # Remove container
docker rm -f NAME              # Force remove
docker exec -it NAME bash      # Enter container
docker logs NAME               # View logs
docker logs -f NAME            # Follow logs
docker stats                   # Resource stats
docker inspect NAME            # Inspect details
docker cp FILE NAME:/path      # Copy to container
docker cp NAME:/path FILE      # Copy from container

# ============ VOLUME ============
docker volume create NAME      # Create volume
docker volume ls               # List volumes
docker volume rm NAME          # Remove volume
docker volume prune            # Remove unused

# ============ NETWORK ============
docker network create NAME     # Create network
docker network ls              # List networks
docker network rm NAME         # Remove network
docker network connect NET CON # Connect container
docker network inspect NAME    # Inspect network

# ============ COMPOSE ============
docker compose up -d           # Start services
docker compose down            # Stop services
docker compose build           # Build services
docker compose logs -f         # View logs
docker compose ps              # List services
docker compose exec SVC bash   # Enter service

# ============ SYSTEM ============
docker system df               # Disk usage
docker system prune -a         # Cleanup all
docker info                    # System info
docker version                 # Version info
```

***

> 💡 **Tips:**
>
> * Selalu gunakan **non-root user** dalam container
> * Gunakan **multi-stage build** untuk memperkecil image
> * Tambahkan **health check** pada setiap service
> * Gunakan **.dockerignore** untuk mempercepat build
> * Simpan **secrets** menggunakan Docker Secrets, bukan environment variable
