# 🐳 Introduction to Containers & Docker - A Beginner's Guide

## 📦 What is a Container?

A **container** is a lightweight, standalone, and executable package that includes everything needed to run a piece of software — such as the **code**, **runtime**, **system tools**, **libraries**, and **settings**.

> 💡 **Analogy:**  
> Think of a container like a sealed **lunchbox**.  
> It has your meal (app), spoon (dependencies), and napkin (settings).  
> No matter where you take it — home, office, or an airplane — just open and eat. It always works the same way!

---

## 🐍 Docker Container Example – Python App

Let’s say you have a Python application that requires Python 3.11 and some libraries like Flask.

### ❌ Traditional Way (Without Docker):
- Manually install Python.
- Install Flask using pip.
- Cross fingers that it works the same on all systems.

### ✅ Docker Way:
You define everything in a `Dockerfile`, so your app runs *exactly* the same everywhere.

```dockerfile
# Use the official Python 3.11 image
FROM python:3.11

# Set the working directory inside the container
WORKDIR /app

# Copy application files into the container
COPY . .

# Install dependencies
RUN pip install -r requirements.txt

# Run the application
CMD ["python", "app.py"]
```

> 🔒 **Benefit**: Everything runs inside the container — isolated from your host machine.

---

## 🚀 Docker Commands Cheat Sheet

### 1️⃣ Basic Docker Commands

| Command | Description |
|--------|-------------|
| `docker version` | Show installed Docker version |
| `docker info` | Show system-wide Docker info |
| `docker help` | List all Docker commands |

---

### 2️⃣ Working with Images

| Command | Description |
|--------|-------------|
| `docker pull <image>` | Download an image from Docker Hub |
| `docker images` | List all downloaded images |
| `docker build -t <name> .` | Build an image from Dockerfile |
| `docker rmi <image>` | Remove an image |

---

### 3️⃣ Working with Containers

| Command | Description |
|--------|-------------|
| `docker run <image>` | Run a container |
| `docker run -it <image> /bin/bash` | Run interactively with a terminal |
| `docker run -d <image>` | Run in detached mode |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers (including stopped) |
| `docker stop <container_id>` | Stop a container |
| `docker start <container_id>` | Start a stopped container |
| `docker restart <container_id>` | Restart a container |
| `docker rm <container_id>` | Remove a container |
| `docker exec -it <container_id> /bin/bash` | Open terminal inside a container |
| `docker logs <container_id>` | View container logs |

---

### 4️⃣ Volumes & Data Sharing

| Command | Description |
|--------|-------------|
| `docker volume create <volume_name>` | Create a volume |
| `docker volume ls` | List all volumes |
| `docker run -v volume_name:/data <image>` | Attach volume to a container |
| `docker volume rm <volume_name>` | Remove a volume |

---

### 5️⃣ Docker Networks

| Command | Description |
|--------|-------------|
| `docker network ls` | List networks |
| `docker network create <network_name>` | Create a new network |
| `docker network rm <network_name>` | Delete a network |

---

### 6️⃣ Dockerfile & Image Management

| Command | Description |
|--------|-------------|
| `docker build -t <image_name> .` | Build image from Dockerfile |
| `docker tag <image_id> <name:tag>` | Tag an image |
| `docker save -o image.tar <image>` | Save image to tar archive |
| `docker load -i image.tar` | Load image from tar file |

---

### 7️⃣ Docker Hub / Registry

| Command | Description |
|--------|-------------|
| `docker login` | Log in to Docker Hub |
| `docker tag <image> username/repo:tag` | Tag image for Docker Hub |
| `docker push username/repo:tag` | Push image to Docker Hub |
| `docker pull username/repo:tag` | Pull image from Docker Hub |

---

### 8️⃣ Docker Compose

| Command | Description |
|--------|-------------|
| `docker-compose up` | Start all services |
| `docker-compose up -d` | Start services in detached mode |
| `docker-compose down` | Stop and remove services |
| `docker-compose ps` | List running services |
| `docker-compose logs` | View service logs |

---


### ✅ Docker Networking commands
docker run -d --name app --network bridge -p 8080:80 nginx     
## Bridge host network. It is like an apt building
docker run --network host nginx       
## Host network. It is like moving into a main house with no reception. easy access but less secure
docker run --network none busybox   
## You live in a completely isolated bunker. None network
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' my_container
## To find the docker container ip address which talk to internal containers. Not connected to external port.
ping host.docker.internal   or ipconfig  
## this will give the docker IPv4 address, which talks to external world.
 
## Note: Every container will have the same 8080:80 port only. Ex: container-1 port 8080:80, container-2 port 8081
like this we can differentiate the containers network. 

Made with 💙 for beginners exploring Docker & Containers.
