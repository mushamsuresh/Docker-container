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
| `docker network create --driver <driver-name> <bridge-name>` | create our own docker network and can deploy our containers in it.  |
| `docker network connect <network-name> <container-name or id>` |  connect a running Docker Container to an existing Network |
| `docker network disconnect <network-name> <container-name>` | Remove a Container from the Network. |
| `docker network prune` | Remove all the unused Docker Networks |
| `docker run -dit --name mycontainer --network demo-network ubuntu` | Run the container and attach it to the network |
| `docker inspect e43da96be2de  findstr -i "NetworkMode" ` | You can check the network mode of a container |
## If it shows "NetworkMode": "host" — you need to recreate the container using bridge or custom network

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
| Command       | Description                                                                                                                                           |   |          |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | - | -------- |
| `FROM`        | **Sets the base image** for your Docker image. Every Dockerfile must start with this.<br>📌 Example: `FROM ubuntu:20.04`                              |   |          |
| `LABEL`       | Adds metadata (like version, description).<br>📌 Example: `LABEL maintainer="you@example.com"`                                                        |   |          |
| `ENV`         | Sets environment variables.<br>📌 Example: `ENV APP_ENV=production`                                                                                   |   |          |
| `RUN`         | Executes a command **during the image build** (layered).<br>📌 Example: `RUN apt-get update && apt-get install -y nginx`                              |   |          |
| `COPY`        | Copies files from your **local machine to the image**.<br>📌 Example: `COPY app.py /app/`                                                             |   |          |
| `ADD`         | Like `COPY` but also supports downloading URLs and extracting `.tar` files.<br>📌 Example: `ADD https://example.com/file.zip /files/`                 |   |          |
| `WORKDIR`     | Sets the **working directory** for all subsequent instructions.<br>📌 Example: `WORKDIR /app`                                                         |   |          |
| `CMD`         | Defines the **default command** to run when a container starts.<br>📌 Example: `CMD ["python", "app.py"]`                                             |   |          |
| `ENTRYPOINT`  | Configures a container to run as an executable with fixed command.<br>📌 Example: `ENTRYPOINT ["nginx", "-g", "daemon off;"]`                         |   |          |
| `EXPOSE`      | Documents the **port** the container listens on (for information only).<br>📌 Example: `EXPOSE 80`                                                    |   |          |
| `VOLUME`      | Creates a mount point for **persistent or shared storage**.<br>📌 Example: `VOLUME /data`                                                             |   |          |
| `USER`        | Specifies the user to run commands inside the container.<br>📌 Example: `USER appuser`                                                                |   |          |
| `ARG`         | Defines build-time variables (used with `--build-arg`).<br>📌 Example: `ARG VERSION=1.0`                                                              |   |          |
| `HEALTHCHECK` | Defines a command to check if the container is still healthy.<br>📌 Example: \`HEALTHCHECK CMD curl --fail [http://localhost:80](http://localhost:80) |   | exit 1\` |
| `SHELL`       | Changes the shell used for `RUN` instructions.<br>📌 Example: `SHELL ["powershell", "-command"]` (for Windows images)                                 |   |          |
| `ONBUILD`     | Adds a trigger instruction to be executed when the image is used as a base.<br>📌 Example: `ONBUILD COPY . /app`                                      |   |          |

# 1. Choose a base image (depends on language/runtime)
FROM <base-image>

# 2. Set the working directory inside the container
WORKDIR /app

# 3. Set environment variables if needed (optional)
ENV PATH /app/node_modules/.bin:$PATH

# 4. Copy dependency descriptor files
COPY <dependency-file(s)> ./

# 5. Install dependencies
RUN <install-command>

# 6. Copy the rest of your project files
COPY . ./

# 7. Expose the port your app uses (optional)
EXPOSE <port-number>

# 8. Define the startup command
CMD ["<command-to-start-app>"]

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
