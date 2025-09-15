### 🚀 Docker Restart Policies

Docker **restart policies** are used to **automatically restart containers** under specific conditions—like on failure or when the Docker daemon restarts. This helps improve **container resilience and uptime**, especially in production environments.

**🔧 Syntax docker**     run --restart <policy> <image>F

### 🔄 Restart Policy Types

| Policy                 | Behavior                                                                 |
|------------------------|--------------------------------------------------------------------------|
| `no` *(default)*       | Do **not restart** the container automatically.                          |
| `always`               | **Always restart** the container if it stops. Even restarts on daemon restart. |
| `on-failure[:max-retries]` | Restart only **if container exits with non-zero exit code**. Optional retry limit. |
| `unless-stopped`       | Like `always`, but **won’t restart** if manually stopped by user.       |

---
### ✅ Examples
### 🔄 Docker Restart Policies

| Policy | Command |
|--------|---------|
| **1. No Restart (Default)** | `docker run --restart no nginx` |
| **2. Always Restart** | `docker run --restart always nginx` |
| **3. On Failure (with Retry Limit)** | `docker run --restart on-failure:3 myapp` |
| **4. Unless Stopped** | `docker run --restart unless-stopped redis` |

### 🔍 Check Restart Policy of a Container

docker inspect --format='{{.HostConfig.RestartPolicy.Name}}' <container_name_or_id>
  
---

## 📌 Recommended Usage

| Use Case                        | Recommended Policy     |
|---------------------------------|------------------------|
| Production application          | `always` or `unless-stopped` |
| Dev/Test with occasional crashes| `on-failure[:max-retries]` |
| Manual testing/debugging        | `no`                   |

---
### ⚠️ Note
- Restart policies **don’t apply** if you use `docker stop` (except with `always`).
- Containers managed by **Kubernetes or Docker Swarm** have their own restart control logic.
### Here's how to create a Docker container with your specified configurations:
🧱 Docker Command
**docker volume create vol1**

docker run -d \
  --name my-container \
  --restart always \
  --network bridge \
  -p 8081:8081 \
  -v vol1:/app/data \
  nginx

