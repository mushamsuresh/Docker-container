
# ✅ Docker Setup and Web Deployment on EC2 using Terraform & Docker

---

## 🚀 Step 1: Launch an EC2 Linux Instance with Existing Key Pair using Terraform

1. Use Terraform to create a Linux EC2 instance.
2. Ensure you reference your **existing key pair** (so you can SSH into it).

---

## 🔐 Step 2: Connect to EC2 from Your Local Machine

```bash
ssh -i /path/to/your-key.pem ec2-user@<EC2-PUBLIC-IP>
```

Replace `<EC2-PUBLIC-IP>` with your actual EC2 instance public IP.

---

## 🐳 Step 3: Install Docker in Amazon Linux 2023

Since we're using **Amazon Linux 2023**, use the following commands:

```bash
sudo dnf update -y                     # Update packages
sudo dnf install docker -y            # Install Docker

If it says “No match for argument: docker”, add the Docker repository first:
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io -y

sudo systemctl start docker           # Start Docker service
sudo systemctl enable docker          # Enable Docker to start on boot
sudo usermod -aG docker ec2-user      # Add ec2-user to Docker group (optional but recommended)
```

Now **log out and log back in** to apply the group change:

```bash
exit
```

Then reconnect via SSH and confirm Docker access:

```bash
groups
```

You should see:
```
ec2-user wheel docker
```

> 🔐 This avoids needing `sudo` every time you run Docker.

---

## ✅ Step 4: Test Docker Installation

```bash
docker run hello-world
```

Expected output:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

---

## 🛠️ Step 5: Create a Dockerized Nginx App

### 1. Create a working directory:
```bash
mkdir myapp && cd myapp
```

### 2. Create an `index.html`:
```bash
echo "<h1>Hello from Dockerized Nginx!</h1>" > index.html
```

### 3. Create a Dockerfile:
```Dockerfile
FROM nginx
COPY index.html /usr/share/nginx/html/
```

To create and edit:
```bash
vi Dockerfile
# Press 'i' to insert, paste content, then press 'Esc', type ':wq' and hit Enter.
```

---
### 4. Copy a `File from local machine`:   Exit form ec2 and connect form local machine
```bash
 scp -i "C:\Users\sures\Downloads\linux_machine_key.pem" -r "C:\Users\sures\Downloads\employee_biodata_form" ec2-user@3.95.223.245:/home/ec2-user/
```

## 🧱 Step 6: Build and Run Docker Container

```bash
docker build -t myapp .     ## build a image
docker run -d -p 80:80 myapp   ## create a container and run
```

---

## 🧪 Step 7: Verify Everything Works

### ✅ Check if container is running:
```bash
docker ps
```

You should see something like:
```
CONTAINER ID   IMAGE   ...   PORTS                NAMES
xxxxxxxxxxxx   myapp   ...   0.0.0.0:80->80/tcp   ...
```

### ✅ Check HTML inside container:
```bash
docker exec -it <container_id> bash
cat /usr/share/nginx/html/index.html
```

---

## 🔓 Step 8: Open EC2 Port 80 to Internet

Go to **EC2 Dashboard → Security Groups → Inbound Rules**, and add:

| Type  | Protocol | Port | Source     |
|-------|----------|------|------------|
| HTTP  | TCP      | 80   | 0.0.0.0/0  |

---

## 🌐 Step 9: View in Browser

1. Get your EC2 public IP:
   ```bash
   curl http://checkip.amazonaws.com
   ```

2. Open in browser:
   ```
   http://<your-ec2-ip>
   ```

You should see your HTML page served by Nginx from Docker! 🎉
---------------------------------------------------------------------------------------------------------------------
What should a docker file contain:

| 🔤 **Type**    | 💡 **Purpose**                                                                | ✅ **Example**                                   |
| -------------- | ----------------------------------------------------------------------------- | ----------------------------------------------- |
| **FROM**       | Specifies the **base image** for the container.                               | `FROM ubuntu:20.04`<br>`FROM openjdk:17`        |
| **LABEL**      | Adds **metadata** like maintainer info.                                       | `LABEL maintainer="suresh@example.com"`         |
| **ENV**        | Sets **environment variables**.                                               | `ENV APP_ENV=production`                        |
| **WORKDIR**    | Sets the **working directory** inside the container.                          | `WORKDIR /app`                                  |
| **COPY**       | **Copies** files from host to image.                                          | `COPY app.jar /app/app.jar`                     |
| **ADD**        | Like `COPY` but supports **remote URLs** and **unzipping** archives.          | `ADD https://example.com/app.zip /app/`         |
| **RUN**        | Executes **commands during build** (installs software, updates system).       | `RUN apt-get update && apt-get install -y curl` |
| **EXPOSE**     | Informs Docker which **port the container listens** on (does not publish it). | `EXPOSE 8080`                                   |
| **CMD**        | Specifies the **default command** to run when container starts.               | `CMD ["java", "-jar", "app.jar"]`               |
| **ENTRYPOINT** | Sets a **fixed executable** for the container.                                | `ENTRYPOINT ["python3", "script.py"]`           |
| **VOLUME**     | Creates a **mount point** for persistent or shared data.                      | `VOLUME /data`                                  |
| **USER**       | Specifies the **user** to run container processes as.                         | `USER appuser`                                  |

