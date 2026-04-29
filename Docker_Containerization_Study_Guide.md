# 🐳 Docker & Containerization — Exam Study Guide

> **Course:** 2110423 Software Engineering
> **Format:** Obsidian-compatible Markdown
> **Exam in:** 3 days

---

## 📚 Table of Contents

- [[#1. Core Concepts]]
- [[#2. Mechanisms & How Things Work]]
- [[#3. Comparisons & Trade-offs]]
- [[#4. Common Misconceptions & Traps]]
- [[#5. High-Probability Exam Questions]]
- [[#6. One-Page Cheat Sheet Summary]]

---

## 1. Core Concepts

### Docker
> An open-source platform that automates the **deployment, scaling, and management** of applications using **containerization**. It packages an application and its dependencies into a portable, runnable unit.

### Containerization
> A method of OS-level virtualization that **isolates application processes** into self-contained units (containers) that share the host OS kernel but run independently in user space.

### Dockerfile
> A **plain-text instruction file** containing a series of commands (e.g., `FROM`, `RUN`, `COPY`, `CMD`) used to **assemble a Docker image**. It defines the build environment for the software.

### Docker Image
> A **read-only, layered template** built from a Dockerfile. It contains the application code, runtime, libraries, environment variables, and configuration files needed to run the application. Images are **immutable** once built.

### Image Layer
> Each instruction in a Dockerfile creates a **new read-only layer** on top of the previous one. Layers are cached and reused to speed up subsequent builds. The layer structure enables efficient storage and sharing.

### Docker Container
> A **runnable, live instance** of a Docker image. A container adds a thin **read-write layer** (container layer) on top of the read-only image layers. It is isolated, lightweight, and portable. Containers are **ephemeral** by default — data is lost when a container is deleted unless a volume is used.

### Docker Hub
> A **public cloud-based registry** (repository) for Docker images. Developers can pull official images (e.g., `postgres`, `ubuntu`, `nginx`) or push their own images. Located at `https://hub.docker.com`.

### Docker Engine (Docker Daemon)
> The **background service** (`dockerd`) running on the host machine that manages building, running, and distributing Docker containers. It listens for Docker API requests from the Docker client (`docker` CLI).

### Docker Compose
> A tool for **defining and running multi-container Docker applications** using a `docker-compose.yml` file. It orchestrates multiple services (e.g., frontend, API, database) with a single command.

### Volume (Named Volume)
> A **Docker-managed persistent storage mechanism** that exists independently of the container lifecycle. Data written to a named volume **survives container deletion**. Best practice for persisting database data.

### Bind Mount
> A **direct mapping** of a specific file or directory from the **host machine's filesystem** into a container path. Used primarily for **live code syncing during development**. Unlike named volumes, the host path is explicitly specified.

### Docker Scout
> A Docker security tool for **analyzing image vulnerabilities** (CVEs), evaluating policy compliance, and sharing security analysis with team members. Uses CVSS scores to classify vulnerabilities as Low, Medium, High, or Critical.

### Multi-Stage Build
> A Dockerfile pattern using **multiple `FROM` instructions** (named stages) to separate the build environment from the final runtime environment. The final image only copies the necessary artifacts (e.g., compiled binaries), resulting in a **significantly smaller and more secure production image**.

### Healthcheck
> A `docker-compose.yml` / Dockerfile directive that configures Docker to **periodically test whether a container is still functioning correctly**. It defines the test command, `interval`, `timeout`, `retries`, and `start_period`.

### Port Forwarding (Port Mapping)
> The mechanism by which a **host machine port is mapped to a container's internal port** using the `-p <host_port>:<container_port>` flag. This allows external traffic to reach a containerized service.

### Docker Desktop
> A GUI application for Mac and Windows that includes the **Docker Engine, Docker CLI, Docker Compose, and a visual dashboard** for managing containers, images, and volumes.

### `dive`
> A third-party CLI tool for **exploring Docker image layers**, inspecting layer contents, and identifying wasted space to optimize image size.

### Hypervisor
> Software that creates and runs **Virtual Machines (VMs)** by abstracting physical hardware. Examples include VMware and VirtualBox. Contrasts with Docker's approach of abstracting the application layer.

---

## 2. Mechanisms & How Things Work

### 2.1 The Docker Build & Run Pipeline

```
Dockerfile ──[docker build]──► Docker Image ──[docker run]──► Docker Container
                                     │
                               [docker push]
                                     │
                              Docker Hub / Registry
                                     │
                               [docker pull]
                                     │
                     (pulled back to run on any Docker-enabled host)
```

**Step-by-step:**
1. Developer writes a `Dockerfile` with build instructions.
2. `docker build -t <image_name> .` is executed — Docker daemon reads each instruction, creates a layer, and caches it.
3. The resulting **Docker image** (read-only) is stored locally.
4. `docker images` lists all locally available images.
5. `docker run -p <host>:<container> <image_name>` instantiates a **container** from the image, adding a writable container layer.
6. `docker push` uploads the image to a registry (e.g., Docker Hub).
7. On another machine, `docker pull` downloads the image; `docker run` starts it.

---

### 2.2 Image Layer Caching Mechanism

Docker builds images **layer by layer**. Each Dockerfile instruction generates one layer. Layers are **cached after the first build**.

**Cache Invalidation Rule:** If a layer's instruction or its inputs **change**, that layer AND **all subsequent layers** are invalidated and rebuilt from scratch.

**Unoptimized example (bad):**
```dockerfile
FROM golang:1.20-alpine
WORKDIR /src
COPY . .              # ← Layer 3: ANY source file change invalidates this
RUN go mod download   # ← Layer 4: Rebuilt every time, even if go.mod unchanged
RUN go build ...
```

**Optimized example (good — copy dependency manifests first):**
```dockerfile
FROM golang:1.20-alpine
WORKDIR /src
COPY go.mod go.sum .  # ← Layer 3: Only invalidated if go.mod or go.sum changes
RUN go mod download   # ← Layer 4: Cached unless go.mod changes ✅
COPY . .              # ← Layer 5: Only invalidated if source code changes
RUN go build ...
```

**Key principle:** Place instructions that change **least frequently** at the **top** of the Dockerfile.

---

### 2.3 Multi-Stage Build Process

```dockerfile
# Stage 1: Builder (Workshop) — has all build tools
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install          # Includes devDependencies
COPY . .
RUN npm run build        # Produces /app/dist

# Stage 2: Final (Showroom) — minimal runtime image
FROM nginx:1.23-alpine   # Tiny, no Node.js, no devDependencies
EXPOSE 80
COPY --from=builder /app/dist /usr/share/nginx/html  # Only copy artifacts
```

**Why it matters:** The final image contains **only Nginx + the built static files**, NOT the Node.js runtime, `node_modules`, source code, or dev tools. This dramatically reduces image size and **attack surface**.

---

### 2.4 Container Lifecycle (State Machine)

```
[docker create] ──► Created
                         │
                    [docker start]
                         │
                         ▼
                      Running ◄──────────────────────────────┐
                         │                                    │
                  [docker pause]                        [docker unpause]
                         │                                    │
                         ▼                                    │
                      Paused ─────────────────────────────────┘
                         │
                  [docker stop]
                         │
                         ▼
                      Stopped
                         │
                     [docker rm]
                         │
                         ▼
                      Deleted
```

> **Note:** `docker run` = `docker create` + `docker start` in one command.

---

### 2.5 How `docker run` with Registry Works

When you run `docker run -it ubuntu`:
1. **Docker Client** sends the command to the **Docker Daemon** on the host.
2. The **Docker Daemon** checks local image cache for `ubuntu`.
3. If not found locally → contacts **Docker Hub** (public registry).
4. **Downloads** the image and stores it in local cache.
5. **Creates** a new container from the ubuntu image.
6. **Starts** the container (interactive terminal with `-it`).

---

### 2.6 Docker Compose Multi-Service Orchestration

```yaml
# docker-compose.yml
version: '3.8'
services:
  frontend:
    build: ./frontend       # Build from local Dockerfile
    ports:
      - "5173:5173"         # host:container port mapping
    volumes:
      - ./frontend:/app     # Bind mount for live reload
    networks:
      - app-network
    depends_on:
      api:
        condition: service_healthy  # Wait for API healthcheck to pass

  api:
    build: ./api
    environment:
      - DB_HOST=db          # Container name used as hostname
    networks:
      - app-network

  db:
    image: postgres:15-alpine  # Pull from Docker Hub directly
    volumes:
      - db-data:/var/lib/postgresql/data  # Named volume for persistence
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U myuser -d mydb"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  db-data:  # Declare named volume at top level

networks:
  app-network:
```

**Key command:** `docker compose up --build` — builds all images and starts all services.

---

### 2.7 How Volumes Solve Container Ephemerality

- Containers are **ephemeral** — the writable container layer is destroyed with `docker rm`.
- Databases require **persistent storage** across container restarts/deletions.
- **Named volumes** are managed by Docker and stored on the host filesystem outside the container. They **survive container deletion**.
- **Bind mounts** directly sync a host directory into the container (used for development hot-reloading).

---

### 2.8 Docker in the DevSecOps Lifecycle

| Stage | How Docker Is Used | Security Benefit |
|---|---|---|
| **1. Code** | Write `Dockerfile` alongside application code | Security-as-code: enforce non-root users, approved base images from day one |
| **2. Build** | CI/CD runs `docker build` automatically | Automated CVE scanning (Trivy, Snyk, Clair) — fail the build on high-severity vulnerabilities |
| **3. Test** | Run containers for unit, integration, DAST security tests | Consistent, production-identical environment for security testing |
| **4. Release** | Tag and push signed image to secure registry | Supply chain integrity: `docker trust sign` guarantees image authenticity |
| **5. Deploy** | Orchestrator (Kubernetes) pulls and runs specific image version | Immutable deployments — no config drift; secrets injected at runtime |
| **6. Operate** | Orchestrator manages scaling, self-healing, networking | Isolation: breach is **contained** within the container sandbox ("blast radius" limitation) |
| **7. Monitor** | Collect logs/metrics from containers to centralized platforms | Centralized visibility (ELK, Splunk, Datadog) |

---

## 3. Comparisons & Trade-offs

### 3.1 Containers vs. Virtual Machines

| Dimension | Containers (Docker) | Virtual Machines |
|---|---|---|
| **Abstraction Level** | Application layer | Physical hardware layer |
| **OS Sharing** | Share host OS kernel | Each VM has its own full Guest OS |
| **Size** | Megabytes (lightweight) | Gigabytes (heavy) |
| **Startup Time** | Seconds | Minutes |
| **Performance Overhead** | Very low (near-native) | Higher (hypervisor overhead) |
| **Isolation Level** | Process-level (namespace/cgroup) | Full hardware-level isolation |
| **Portability** | High (runs anywhere Docker is installed) | Moderate (depends on hypervisor) |
| **Security Boundary** | Weaker (shares kernel) | Stronger (fully separate OS) |
| **Use Case** | Microservices, CI/CD, scalable deployments | Testing different OSes, strong isolation, legacy apps |
| **Examples** | Docker | VMware, VirtualBox |

### 3.2 Named Volume vs. Bind Mount

| Dimension | Named Volume | Bind Mount |
|---|---|---|
| **Managed by** | Docker | Host OS / Developer |
| **Path specification** | Docker-assigned (abstract name) | Explicit host filesystem path |
| **Primary use case** | **Persisting production data** (databases) | **Development** — live code syncing |
| **Cross-platform compatibility** | Excellent | Can have issues (e.g., `node_modules` conflicts on Mac vs. Linux) |
| **Performance** | Optimized by Docker for I/O | May vary by OS |
| **Backup/migrate** | Easy with Docker commands | Manual |
| **Syntax example** | `db-data:/var/lib/postgresql/data` | `./frontend:/app` |

### 3.3 Dockerfile Instruction Trade-offs

| Instruction | Purpose | Trade-off / Note |
|---|---|---|
| `FROM` | Set base image | Use minimal images (alpine) to reduce size and attack surface |
| `RUN` | Execute shell command, creates new layer | Combine `RUN` commands with `&&` to minimize layers |
| `COPY` | Copy files into image | Invalidates cache for all subsequent layers if files change |
| `ADD` | Like COPY but supports URLs/archives | Prefer `COPY` for predictability |
| `CMD` | Default command when container starts | Can be overridden at `docker run` |
| `ENTRYPOINT` | Fixed executable for the container | Combined with `CMD` for flexible but defined startup |
| `EXPOSE` | Documents the port the container listens on | Does NOT actually publish the port — use `-p` flag for that |
| `ENV` | Set environment variables | Visible in image metadata — don't store secrets here |
| `WORKDIR` | Set working directory | Creates directory if it doesn't exist |
| `USER` | Set the user to run as | Use non-root for security (principle of least privilege) |

---

## 4. Common Misconceptions & Traps

> ⚠️ These are high-priority traps for multiple-choice questions.

### Trap 1: `EXPOSE` does NOT publish the port
- **Misconception:** Adding `EXPOSE 5173` in a Dockerfile makes the service accessible from the host.
- **Reality:** `EXPOSE` is **documentation only**. You must use `-p 5173:5173` in `docker run` or `ports:` in `docker-compose.yml` to actually map the port.

### Trap 2: Containers are NOT the same as Images
- **Misconception:** "Run the image" and "run the container" mean the same thing.
- **Reality:** An **image** is a read-only template. A **container** is a running instance of an image. You can run **multiple containers** from the **same image** simultaneously.

### Trap 3: Container data is NOT persistent by default
- **Misconception:** Data written inside a container persists after `docker rm`.
- **Reality:** The container layer (writable) is destroyed when the container is removed. **Volumes** are required for persistence.

### Trap 4: `docker run` ≠ `docker start`
- **Misconception:** `docker start` creates and runs a new container.
- **Reality:** `docker start` **restarts a stopped existing container**. `docker run` = `docker create` + `docker start` (creates a **new** container from an image).

### Trap 5: Layer caching — cache is NOT invalidated at the changed layer only
- **Misconception:** Only the changed layer is rebuilt.
- **Reality:** The changed layer AND **all layers after it** are invalidated. This is why instruction **ordering matters**.

### Trap 6: `depends_on` in Docker Compose does NOT guarantee service readiness
- **Misconception:** `depends_on: db` means the API waits for the database to be fully ready.
- **Reality:** `depends_on` by default only waits for the container to **start**, not for the service inside to be ready. You need `condition: service_healthy` with a defined **healthcheck** for true readiness.

### Trap 7: Named volumes and bind mounts are NOT the same
- **Misconception:** Both are just ways to persist data.
- **Reality:** Named volumes are **Docker-managed** (opaque path, best for production data). Bind mounts **expose the host filesystem** directly (best for development, can cause platform-specific conflicts).

### Trap 8: `ENV` variables in Dockerfiles are NOT secrets
- **Misconception:** You can safely put passwords in `ENV` instructions in a Dockerfile.
- **Reality:** `ENV` values are **visible in image metadata** (`docker inspect`). Secrets should be injected at **runtime** (e.g., Docker secrets, environment variable files not committed to git).

### Trap 9: `FROM node` ≠ `FROM node:alpine`
- **Misconception:** Base image tags are interchangeable.
- **Reality:** `FROM node` pulls the **latest full image** (can be 1GB+). `FROM node:22-alpine` pulls a **minimal Alpine-based image** (~170MB). Always pin to a specific version for reproducibility and use alpine for smaller images.

### Trap 10: Containers sharing the host kernel is a security consideration
- **Misconception:** Containers provide the same isolation as VMs.
- **Reality:** Containers share the **host OS kernel**. A kernel-level exploit can potentially escape the container. VMs have stronger isolation because each has its own Guest OS.

---

## 5. High-Probability Exam Questions

### Q1
**Which of the following BEST describes the relationship between a Docker image and a Docker container?**

- A) A container is a compressed archive of an image stored on disk.
- B) An image is a running instance of a container.
- C) A container is a running instance of an image, adding a writable layer on top. ✅
- D) An image and a container are identical; the terms are interchangeable.

> **Explanation:**
> - A: Incorrect — an image is stored as layers, not as a compressed archive.
> - B: Incorrect — this reverses the relationship.
> - C: ✅ Correct — a container is the live instantiation of an image, with an added R/W container layer.
> - D: Incorrect — they are fundamentally different concepts.

---

### Q2
**A developer modifies a JavaScript source file and rebuilds the following Dockerfile. Which layers will be rebuilt?**

```dockerfile
FROM node:22-alpine      # Layer 1
WORKDIR /app             # Layer 2
COPY package.json ./     # Layer 3
RUN npm install          # Layer 4
COPY . .                 # Layer 5  ← source file change invalidates here
EXPOSE 5173              # Layer 6
CMD ["npm", "run", "dev"] # Layer 7
```

- A) Only Layer 5
- B) Layers 5 and 6 only
- C) Layers 5, 6, and 7 ✅
- D) All 7 layers

> **Explanation:**
> - A/B: Incorrect — cache invalidation is **cascading**. Once Layer 5 is invalidated, all subsequent layers must also be rebuilt.
> - C: ✅ Correct — Layers 1–4 are cached (no change to base image, workdir, or package.json). Layer 5 is invalidated by the source change, and Layers 6–7 follow.
> - D: Incorrect — Layers 1–4 remain cached because their inputs haven't changed.

---

### Q3
**What is the PRIMARY purpose of the `EXPOSE` instruction in a Dockerfile?**

- A) It maps a host machine port to the container's internal port.
- B) It configures the firewall to allow traffic on the specified port.
- C) It documents which port the application inside the container listens on. ✅
- D) It automatically publishes the port when `docker compose up` is run.

> **Explanation:**
> - A: Incorrect — that is the job of `-p <host>:<container>` in `docker run` or `ports:` in compose.
> - B: Incorrect — `EXPOSE` has no effect on the firewall.
> - C: ✅ Correct — `EXPOSE` is metadata/documentation only. It does not publish or bind any port.
> - D: Incorrect — `ports:` in `docker-compose.yml` is required for publishing.

---

### Q4
**Which scenario would call for a NAMED VOLUME rather than a BIND MOUNT?**

- A) A developer wants changes to their React source code to appear immediately inside the container without rebuilding the image.
- B) A PostgreSQL database container needs its data to persist even after the container is deleted and recreated. ✅
- C) A developer needs to share a specific host config file with a container for local testing.
- D) A CI pipeline needs to mount test fixtures from the repository into a test runner container.

> **Explanation:**
> - A, C, D: All describe scenarios where the host path is specifically known and direct file access is needed → **bind mounts**.
> - B: ✅ Correct — named volumes are Docker-managed, platform-independent, and designed for persisting opaque data like database files across the container lifecycle.

---

### Q5
**In a `docker-compose.yml`, a frontend service uses `depends_on: api: condition: service_healthy`. What does this require?**

- A) The `api` service container must have been created at some point in the past.
- B) The `api` service must be running and its defined healthcheck must be passing. ✅
- C) The `api` service must have its port published to the host.
- D) The `depends_on` keyword ensures the `frontend` starts after the `api` Dockerfile is built.

> **Explanation:**
> - A: Incorrect — `depends_on` checks live status, not past state.
> - B: ✅ Correct — `condition: service_healthy` means Docker waits until the container is running AND its healthcheck reports `healthy` before starting the dependent service.
> - C: Incorrect — port publication has no bearing on healthcheck status.
> - D: Incorrect — `depends_on` governs startup order of containers, not build order.

---

### Q6
**What is the KEY advantage of a Multi-Stage Build in Docker?**

- A) It allows running multiple containers from a single Dockerfile.
- B) It enables parallel execution of all Dockerfile instructions.
- C) It produces a final image that excludes build-time tools and intermediate artifacts, reducing image size and attack surface. ✅
- D) It automatically pushes each stage as a separate image to Docker Hub.

> **Explanation:**
> - A: Incorrect — that is the role of Docker Compose.
> - B: Incorrect — Dockerfile instructions are sequential per stage.
> - C: ✅ Correct — the final stage only copies the necessary build output (e.g., `/app/dist`) from the builder stage, leaving behind compilers, devDependencies, and source code.
> - D: Incorrect — intermediate stages are not pushed automatically.

---

### Q7
**How do containers differ from Virtual Machines in terms of OS architecture?**

- A) Containers include a full copy of the guest OS, while VMs share the host OS kernel.
- B) Containers share the host OS kernel and run as isolated processes; VMs each include a full guest OS. ✅
- C) Both containers and VMs share the host OS kernel, but containers use a hypervisor layer.
- D) VMs are more lightweight than containers because they abstract at the application layer.

> **Explanation:**
> - A: Incorrect — this reverses the correct description.
> - B: ✅ Correct — this is the fundamental architectural difference.
> - C: Incorrect — VMs use a hypervisor; containers do not.
> - D: Incorrect — containers are more lightweight; VMs abstract hardware, not applications.

---

### Q8
**At which stage of the DevSecOps lifecycle does automated CVE (vulnerability) scanning of Docker images MOST critically occur?**

- A) Code — when the developer writes the Dockerfile
- B) Build — when CI/CD runs `docker build` and scans the resulting image ✅
- C) Deploy — when Kubernetes pulls the image from the registry
- D) Monitor — when logs are collected from running containers

> **Explanation:**
> - A: Incorrect — scanning requires an actual built image to analyze.
> - B: ✅ Correct — the Build stage is the critical security gate where automated tools scan the newly built image for CVEs. Failing the pipeline here prevents a vulnerable image from ever reaching the registry.
> - C: Incorrect — scanning at deploy time is too late; the image is already in the registry.
> - D: Incorrect — monitoring detects runtime anomalies, not pre-deployment vulnerabilities.

---

### Q9
**A developer runs `docker run -p 5172:5173 --name retest2 retest`. What does `5172:5173` mean?**

- A) The container uses port 5172 internally; the host accesses it on port 5173.
- B) The host machine port 5172 is forwarded to the container's internal port 5173. ✅
- C) Both the host and container use port 5172; port 5173 is the fallback.
- D) The container has two services: one on 5172 and one on 5173.

> **Explanation:**
> - A: Incorrect — the format is `<host_port>:<container_port>`, not the reverse.
> - B: ✅ Correct — `host_port:container_port`. Traffic arriving on host port 5172 is forwarded to the container's port 5173.
> - C/D: Incorrect — the two numbers are distinct host and container ports.

---

### Q10
**Which of the following is a Docker BEST PRACTICE for Dockerfile security?**

- A) Use `FROM ubuntu` and install all dependencies manually with `apt-get` for maximum control.
- B) Always use `FROM node` (no tag) so you automatically receive the latest security patches.
- C) Store database passwords directly in `ENV` instructions to ensure they are available at runtime.
- D) Run the application as a non-root user inside the container using the `USER` instruction. ✅

> **Explanation:**
> - A: Incorrect — using a generic base like `ubuntu` adds unnecessary packages; prefer purpose-built official images (`FROM node`).
> - B: Incorrect — using `latest`/no tag is an anti-pattern. It breaks reproducibility and can introduce unexpected breaking changes. Always pin versions.
> - C: Incorrect — `ENV` values are visible in `docker inspect` output. Secrets should be injected at runtime via Docker secrets or runtime environment variables from a `.env` file not committed to source control.
> - D: ✅ Correct — running as a non-root user limits the damage an attacker can do if the container is compromised (principle of least privilege).

---

## 6. One-Page Cheat Sheet Summary

### 🏗️ Core Components
- **Dockerfile** → Instructions to build an image
- **Docker Image** → Read-only layered template (immutable)
- **Docker Container** → Running instance of an image (adds R/W layer)
- **Docker Hub** → Public registry for images
- **Docker Compose** → Multi-container orchestration via `docker-compose.yml`

### 🔑 Key Commands
| Command | Action |
|---|---|
| `docker build -t <name> .` | Build image from Dockerfile |
| `docker images` | List local images |
| `docker run -p <h>:<c> <img>` | Run container with port mapping |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers (incl. stopped) |
| `docker stop <id>` | Stop a running container |
| `docker rm <id>` | Delete a stopped container |
| `docker logs <id>` | View container logs |
| `docker compose up --build` | Build and start all services |
| `docker scout quickview` | Scan image for vulnerabilities |
| `dive <image>` | Inspect image layers |

### 📦 Volumes — Know the Difference
| | Named Volume | Bind Mount |
|---|---|---|
| **Use for** | Production data (DB) | Dev code syncing |
| **Syntax** | `db-data:/var/lib/postgresql/data` | `./frontend:/app` |
| **Managed by** | Docker | Developer/Host |

### ⚡ Layer Caching Rule
> Copy **rarely-changed files first** (e.g., `package.json`), **frequently-changed files last** (e.g., source code). A changed layer invalidates **itself + all layers below it**.

### 🆚 Container vs. VM
| | Container | VM |
|---|---|---|
| OS | Shares host kernel | Own Guest OS |
| Size | MB | GB |
| Speed | Seconds | Minutes |
| Isolation | Process-level | Hardware-level |

### 🔒 Security Best Practices
- ✅ Use official, pinned, minimal base images (`node:22-alpine`)
- ✅ Use `USER <non-root>` instruction
- ✅ Use `.dockerignore` to exclude sensitive/unnecessary files
- ✅ Use Multi-Stage Builds to minimize final image size
- ✅ Scan images with `docker scout` / Trivy / Snyk
- ✅ Inject secrets at **runtime**, never in `ENV` in Dockerfile
- ❌ Never use `FROM image:latest` in production
- ❌ `EXPOSE` does NOT publish ports — use `-p` or `ports:`

### 🔄 Container Lifecycle
```
Created → Running → Paused → Running → Stopped → Deleted
  create    start    pause   unpause     stop       rm
                          (docker run = create + start)
```

### 🔗 DevSecOps Integration
- **Build** = Automated CVE scanning (most critical security gate)
- **Deploy** = Immutable deployments, no config drift, runtime secrets injection
- **Operate** = Container = security sandbox, limits "blast radius"

### 🧠 Remember These Traps
1. `EXPOSE` ≠ published port
2. Image ≠ Container (image is template, container is instance)
3. Container data is **ephemeral** — use volumes for persistence
4. `docker run` ≠ `docker start` (run creates new; start restarts existing)
5. Cache invalidation is **cascading** — affects all layers after the changed one
6. `depends_on` alone ≠ service ready — need `condition: service_healthy` + healthcheck
7. Named volume ≠ Bind mount (different use cases)
8. Containers share host kernel → weaker isolation than VMs
