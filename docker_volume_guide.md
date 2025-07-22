
# Docker Volumes - Practice Guide

## 1. Sharing a Named Volume Between Containers

This helps two containers read/write to the same data.

### ✅ Step-by-Step

#### Create and use the volume in container 1:

```bash
docker volume create sharedvol
docker run -it --name containerA --mount source=sharedvol,target=/data ubuntu /bin/bash
```

#### Inside containerA:

```bash
echo "Hello from container A" > /data/hello.txt
exit
```

---

#### Run container 2 with the same volume:

```bash
docker run -it --name containerB --mount source=sharedvol,target=/data ubuntu /bin/bash
```

#### Inside containerB:

```bash
cat /data/hello.txt
```

#### ✅ You’ll see:

```
Hello from container A
```

🎉 **Success! You’ve shared a volume!**

---

## 2. Using a Bind Mount

Bind mounts allow you to mount a folder from your host into a container. This is great for development.

### ✅ Example:

```bash
docker run -it --name devcontainer -v /absolute/path/on/host:/app ubuntu /bin/bash
```

If you're using Windows PowerShell:

```powershell
docker run -it --name devcontainer -v C:\Users\sures\myapp:/app ubuntu /bin/bash
```

### ✅ Inside the container:

```bash
cd /app
ls
```

You will see the files from your host system. Any changes you make here will reflect directly on your host.

---

### 🧹 Cleanup:

```bash
docker container rm -f containerA containerB devcontainer
docker volume rm sharedvol
```

---

Happy Docker practicing! 🐳
