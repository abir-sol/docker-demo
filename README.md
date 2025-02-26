# Docker Technical Session

A hands-on introduction to Docker concepts, workflows, and commands for ML Engineers.

## Agenda

### Part 1: Introduction to Docker Concepts
* What is Docker? Why use it?
* What is containerization? Comparison to traditional deployment
* Key benefits: Consistency, isolation, efficiency, portability, scalability
* Docker architecture: Docker daemon, client, images, containers, Docker Hub
* Common use cases: Development environments, microservices, CI/CD

### Part 2: Docker Fundamentals
* Installation and setup: Verify Docker in Windows & Linux, Docker Desktop
* Basic commands
* Dockerfile basics: Creating custom images
* Image layers: Understanding how Docker optimizes storage

### Part 3: Docker Compose Introduction
* What is Docker Compose? Managing multi-container applications
* docker-compose.yml structure: Services, networks, volumes, env variables
* Basic commands
* Debugging containers

### Part 4: Hands-on Docker Demo Projects
* Demo 1: Pre-built Docker Nginx Website Deployment
* Demo 2: Custom Docker PDF Generation with Python
* Demo 3: Docker Compose Medical Text Extraction Service

### Part 5: Docker for ML Projects

### Part 6: Modern Python Tooling uv
* Comparison of conda, venv, pipenv, poetry, uv
* Benefits of uv
* Basic commands
* Project setup
* Best practices
* Docker with uv

---

## Part 1: Introduction to Docker Concepts

### What is Docker?

Docker is an open-source platform that automates the deployment, scaling, and management of applications using containerization technology. It packages an application and all its dependencies together in the form of a container, ensuring that the application works seamlessly in any environment.

### What is Containerization?

Containerization is a lightweight form of virtualization that packages an application and its dependencies into a standardized unit (container) for software development and deployment.

**Comparison to Traditional Deployment:**

| Traditional Deployment | Containerization |
|------------------------|------------------|
| Requires full OS per application | Shares host OS kernel |
| Heavy resource consumption | Lightweight resource usage |
| Slow to start up | Nearly instant startup |
| Inconsistent environments | Consistent across environments |
| Complex dependency management | Dependencies bundled with application |
| Difficult to scale | Easy to scale horizontally |

### Key Benefits

- **Consistency**: "Works on my machine" problem eliminated
- **Isolation**: Applications run in isolated environments
- **Efficiency**: Uses fewer resources than traditional VMs
- **Portability**: Run anywhere Docker is installed
- **Scalability**: Easily scale containers up or down

### Docker Architecture

![Docker Architecture](https://docker-docs.uclv.cu/engine/images/architecture.svg)

- **Docker Daemon**: Background service running on the host
- **Docker Client**: Command-line interface to interact with Docker
- **Docker Images**: Read-only templates used to create containers
- **Docker Containers**: Runnable instances of images
- **Docker Hub**: Registry service for sharing and finding Docker images

### Common Use Cases

- **Development**: Consistent dev environments across team
- **Microservices**: Deploying and scaling independent services
- **CI/CD**: Integration with continuous delivery pipelines


## Part 2: Docker Fundamentals

### Installation and Setup

#### **Windows Installation**

Docker can be installed easily using Docker Desktop which also comes with a GUI application. It is available for Windows, MacOS, and Linux. Here's how to install in windows:
1. Download Docker Desktop from [docker.com](https://docs.docker.com/desktop/setup/install/windows-install/)
2. Install and enable WSL 2 if prompted
3. Verify installation: `docker --version`

#### **Linux Installation (Ubuntu)**

In Ubuntu, the latest Docker Engine can be intalled using the following commands:
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Install Docker Engine
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Verify installation
docker --version
```

### Basic Commands

#### Image Management

| Command | Description |
|---------|-------------|
| `docker images` | List downloaded images |
| `docker pull <image>` | Download an image |
| `docker push <image>` | Upload an image to registry |
| `docker rmi <image>` | Remove an image |
| `docker build -t <name>:<tag> <path>` | Build image from Dockerfile |
| `docker history <image>` | Show image layer history |
| `docker save <image> > file.tar` | Save image to tar archive |
| `docker load < file.tar` | Load image from tar archive |

#### Container Management

| Command | Description |
|---------|-------------|
| `docker run <image>` | Create and start a container |
| `docker run -d <image>` | Run container in detached mode |
| `docker run -p <host-port>:<container-port> <image>` | Map container port to host port |
| `docker run -v <host-path>:<container-path> <image>` | Mount host directory into container |
| `docker run --name <name> <image>` | Assign a name to the container |
| `docker run --rm <image>` | Remove container when it exits |
| `docker start <container>` | Start a stopped container |
| `docker stop <container>` | Stop a running container |
| `docker restart <container>` | Restart a container |
| `docker pause <container>` | Pause a running container |
| `docker unpause <container>` | Unpause a paused container |
| `docker rm <container>` | Remove a container |
| `docker rm -f <container>` | Force remove a running container |

#### Container Inspection

| Command | Description |
|---------|-------------|
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker logs <container>` | View container logs |
| `docker logs -f <container>` | Follow container logs |
| `docker inspect <container>` | View detailed container info |
| `docker exec -it <container> <command>` | Execute command in running container |
| `docker exec -it <container> bash` | Start a shell in container |
| `docker top <container>` | Display running processes |
| `docker stats` | Display container resource usage |

#### System Commands

| Command | Description |
|---------|-------------|
| `docker info` | Display system-wide information |
| `docker version` | Show Docker version |
| `docker system prune` | Remove unused data |
| `docker system prune -a` | Remove all unused images and containers |

### Dockerfile Basics

A Dockerfile is a text document containing instructions to build a Docker image:

```dockerfile
# Base image
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY src/ .

# Set environment variables
ENV HOST="localhost"
ENV PORT=8080

# Expose port
EXPOSE 8080

# Command to run when container starts
CMD ["python", "app.py"]
```

#### Common Dockerfile Instructions

| Instruction | Description |
|-------------|-------------|
| `FROM` | Base image to build from |
| `WORKDIR` | Set working directory |
| `COPY` | Copy files from host to image |
| `ADD` | Copy files and extract archives |
| `RUN` | Execute commands during build |
| `ENV` | Set environment variables |
| `EXPOSE` | Document container ports |
| `VOLUME` | Create mount point for volumes |
| `CMD` | Default command to run on start |
| `ENTRYPOINT` | Configure container as executable |

### Image Layers

Docker uses a layered filesystem to build images. Each instruction in a Dockerfile creates a new layer:

- Layers are cached and reused for faster builds
- Only changed layers are rebuilt
- Shared layers are stored once
- Best practice: Order instructions from least to most frequently changed

## Part 3: Docker Compose Introduction

### What is Docker Compose?

Docker Compose is a tool for defining and running multi-container Docker applications. It uses a YAML file to configure application services, networks, and volumes.

### docker-compose.yml Structure

```yaml
services:
  web_service:
    build: ./web
    ports:
      - "8000:8000"
    volumes:
      - ./web:/app
    environment:
      - DEBUG=True
    depends_on:
      - db_service
  
  db_service:
    image: postgres:13
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=myapp

volumes:
  postgres_data:

networks:
  default:
    driver: bridge
```

#### Key Components

- **services**: Container definitions
- **networks**: Network configuration
- **volumes**: Persistent data storage
- **environment**: Environment variables
- **depends_on**: Service dependencies

### Basic Commands

| Command | Description |
|---------|-------------|
| `docker compose up` | Create and start all services |
| `docker compose up -d` | Start in detached mode |
| `docker compose down` | Stop and remove all services |
| `docker compose down -v` | Also remove volumes |
| `docker-compose down --rmi all` | Remove all images |
| `docker compose ps` | List containers |
| `docker compose logs` | View output from all services |
| `docker compose logs -f` | Follow log output |
| `docker compose exec <service> <command>` | Execute command in service |
| `docker compose build` | Build or rebuild services |
| `docker compose pull` | Pull service images |
| `docker compose restart` | Restart services |
| `docker compose stop` | Stop services |
| `docker compose start` | Start services |

### Debugging Containers

- **Logs**: `docker logs <container>` or `docker compose logs <service>`
- **Interactive Shell**: `docker exec -it <container> bash`
- **Inspect Processes**: `docker top <container>`
- **Resource Usage**: `docker stats`
- **Network Inspection**: `docker network inspect <network>`
- **Volume Inspection**: `docker volume inspect <volume>`


### Part 4: Hands-on Docker Projects

#### Project 1: Pre-built Docker Nginx Website Deployment
This project is in [1-nginx-website](./1-nginx-website/) directory. Fmore more details on the project see the project [README](1-nginx-website/README.md).

#### Project 2: Custom Docker PDF Generation with Python
This project is in [2-pdf-generator](./2-pdf-generator/) directory. Fmore more details on the project see the project [README](./2-pdf-generator/README.md).

#### Project 3: Docker Compose Medical Text Extraction Service
This project is in [3-text-extractor](./3-text-extractor/) directory. Fmore more details on the project see the project [README](./3-text-extractor/README.md).


## Part 5: Docker for ML Projects

Docker provides several benefits for Machine Learning projects:

- **Model deployment**: Package models with serving infrastructure
- **GPU support**: Access to NVIDIA GPUs with nvidia-docker and easy setup
- **Reproducible environments**: Ensure consistent dependencies
- **Scalable training**: Distribute training across containers
- **ML-specific images**: Pre-built images for TensorFlow, PyTorch, etc.

### Example ML Project Dockerfile

```dockerfile
FROM nvidia/cuda:11.6.2-cudnn8-runtime-ubuntu20.04

WORKDIR /app

# Install Python and dependencies
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Install ML libraries
COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt

# Copy model and code
COPY . .

# Expose port for API
EXPOSE 8000

# Start the model server
CMD ["python3", "serve.py"]
```

## Part 6: Modern Python Tooling uv

### Comparison of Python Package Managers

| Tool | Pros | Cons |
|------|------|------|
| **conda** | Environment + package manager, supports non-Python deps | Slow, heavy |
| **venv** | Built-in, lightweight | Just virtual environments, no package management |
| **poetry** | Modern dependency resolution, builds packages | Complex, sometimes slow |
| **uv** | Ultra-fast, Rust-based, compatible with pip | Newer, fewer features |

### Benefits of uv

- **Speed**: 10-100x faster than pip for large dependency trees
- **Reliability**: Deterministic installations
- **Compatibility**: Drop-in replacement for pip
- **Simplicity**: Clear, focused tool

### Basic Commands

| Command | Description |
|---------|-------------|
| `uv venv` | Create virtual environment |
| `uv pip install <package>` | Install package |
| `uv pip install -r requirements.txt` | Install from requirements file |
| `uv pip freeze` | List installed packages |
| `uv pip compile requirements.in -o requirements.txt` | Generate lockfile-like requirements.txt |
| `uv lock` | Generate uv.lock file using the pyproject.toml file |
| `uv sync` | From pyproject.toml and lock file installs or syncs the dependencies |

### Project Setup

```bash
# Create a new project directory
mkdir my_project
cd my_project

# Create virtual environment
uv venv

# Activate environment
source .venv/bin/activate

# Install packages
uv pip install numpy pandas scikit-learn

# Export requirements
uv pip freeze > requirements.txt
```

### Best Practices

- Use `uv sync` for consistent project builds
- Keep development and production dependencies separate
- Integrate with CI/CD pipelines for faster builds
- Use `Ruff` for Python linter and code formatting

### Docker with uv

```dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install uv
RUN pip install uv

# Copy requirements
COPY requirements.txt .

# Install dependencies with uv (much faster!)
RUN uv pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Run the application
CMD ["python", "app.py"]
```

---

## Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [uv Documentation](https://docs.astral.sh/uv/)
