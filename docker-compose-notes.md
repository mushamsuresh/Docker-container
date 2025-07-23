
# 🐳 Multi-Container Flask + PostgreSQL App (Docker Compose)

## 📚 Overview

This project demonstrates how to use **Docker Compose** to run a **multi-container application** with:
- A **Flask web application**
- A **PostgreSQL database**

---

## 🧱 Project Structure

```
multi-container-app/
│
├── app/
│   ├── app.py              # Flask application
│   ├── requirements.txt    # Python dependencies
│   └── Dockerfile          # Image build instructions
│
├── docker-compose.yml      # Multi-container orchestration
```

---

## 🐳 Docker Containers Used

| Service | Role             | Source Image         | Notes                          |
|---------|------------------|----------------------|--------------------------------|
| `web`   | Flask App        | Custom (`Dockerfile`) | Talks to PostgreSQL via psycopg2 |
| `db`    | PostgreSQL DB    | `postgres:14`        | Stores data in Docker volume   |

---

## ⚙️ Docker Compose Breakdown

### `web` service
- Builds from `./app` folder using `Dockerfile`
- Installs Python and dependencies
- Runs Flask app
- Exposes port `5000` to host
- Depends on `db` (ensures it starts after db)

### `db` service
- Uses official `postgres:14` image
- Configured with:
  - `POSTGRES_DB`: testdb
  - `POSTGRES_USER`: postgres
  - `POSTGRES_PASSWORD`: example
- Persists data in named volume `postgres_data`

---

## 📄 Dockerfile vs docker-compose.yml

| Feature                      | Dockerfile                                  | docker-compose.yml                                       |
|-----------------------------|----------------------------------------------|----------------------------------------------------------|
| **Purpose**                 | Builds a custom Docker image                 | Defines and runs multi-container applications            |
| **Scope**                   | Single container                             | Multiple containers (services)                           |
| **Format**                  | Text file with Docker commands               | YAML configuration file                                  |
| **Common Use**              | Set up application environment               | Configure networking, volumes, and container relationships |
| **Typical Location**        | Inside the project directory (e.g., `app/`)  | In the root project directory                            |
| **Example Instruction**     | `RUN pip install -r requirements.txt`        | `build: ./app`<br>`depends_on: db`                       |

> ✅ Think of `Dockerfile` as **"how to build an image"** and `docker-compose.yml` as **"how to run and connect containers"**.

---

## ✨ Features of Docker Compose

- **Multi-container orchestration** from a single file
- **One-command deployment** (`docker-compose up`)
- Built-in support for **volumes**, **networks**, and **environment variables**
- Easily define **build context** and **service dependencies**
- Support for scaling services (e.g., replicas)
- Compatible with `.env` files for config

---

## ✅ Best Practices for Docker Compose

1. **Use `.env` files** to avoid hardcoding credentials
2. **Use named volumes** for persistent data
3. **Use `depends_on`** to define container startup order (but not for app-level checks)
4. **Separate development and production files** (`docker-compose.override.yml`)
5. **Keep images light** using minimal base images (e.g., `python:3.10-slim`)
6. **Avoid root users** in containers for security
7. **Don’t store secrets** in the `docker-compose.yml` file
8. **Keep services loosely coupled** for flexibility and testing

---

## 👍 Advantages of Docker Compose

- 🧩 Simplifies managing multiple containers
- ⚙️ Automates service start-up and linking
- 🔄 Easily reproducible dev/test environments
- 📁 Uses YAML - simple and human-readable
- 🔄 Supports auto-rebuilds and hot reload in development
- 🗂 Supports version control of infrastructure

---

## 👎 Disadvantages of Docker Compose

- ❌ Not suited for **production orchestration** at scale (use Kubernetes or Docker Swarm instead)
- 🧪 `depends_on` only ensures order, not actual service readiness
- ⚠️ May hide infrastructure complexity for larger applications
- 🧠 Requires Docker knowledge to debug container issues
- ⛔ Not recommended for applications requiring **high availability**

---

## ▶️ How to Run

```bash
docker-compose up --build
```

Visit the app at: [http://localhost:5000](http://localhost:5000)

---

## 💡 Next Steps (Practice Ideas)

- Add a 3rd container: pgAdmin (web UI for DB)
- Add database migrations with Alembic
- Use `.env` file for sensitive config
- Add volume for app code and enable auto-reload
- Scale `web` service using replicas

---

## 🧼 To Stop and Clean Up

```bash
docker-compose down
```

To remove volumes too:

```bash
docker-compose down -v
```

---

## 📂 References

- [Docker Compose Docs](https://docs.docker.com/compose/)
- [PostgreSQL Docker Hub](https://hub.docker.com/_/postgres)
- [Flask Documentation](https://flask.palletsprojects.com/)
