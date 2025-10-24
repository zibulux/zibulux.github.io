---
layout: post
title: "Docker Basics 🧩 — Your Quick Guide to Essential Commands"
date: 2025-10-23
categories: [3-Server, 1Ser-Theory]
tags: [Server, Docker]
author: <author_mpmk>
image:
    path: /assets/img/server/Theory-Docker-CMD.png
---

## ⚙️ Core Principles: **Run – Ship – Build**

At its core, Docker simplifies the workflow of building, shipping, and running applications.  
Everything revolves around three main actions:

1. **Build** → Create lightweight, portable images for your app  
2. **Ship** → Distribute those images (e.g., through Docker Hub or registries)  
3. **Run** → Start isolated containers from images anywhere  

To see all available commands:
```bash
docker
```

To log in to your Docker Hub account:
```bash
docker login
```
---

## 🧩 Image vs Container

An Image is a blueprint — the application and all its dependencies.

A Container is a running instance of that image (a process with its own filesystem and network).

You can launch multiple containers from the same image.

---

## 🔍 What Is an Image?

An image contains:

App binaries and dependencies

Metadata about how to run it (via Dockerfile instructions)

Filesystem layers that represent each modification in its build process

## 🧠 Official definition:
“An image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime.”

>Notes: 
> - Not a full OS → No kernel or drivers.
> - Can be tiny (a static Go binary) or heavy (Ubuntu + Apache + PHP).

---

## 🧱 Image Layers

Every image is built in layers, and Docker caches them for efficiency.

##### View Image History

Shows all layers (and commands) used to build the image:
```bash
docker history <image_name>
```

##### Inspect an Image

Displays detailed JSON metadata (env vars, exposed ports, entrypoints, etc.):
```bash
docker image inspect <image_name>
```

##### Tag an Image

Assigns or renames image tags.

>💡 Default tag is latest, but it does not necessarily mean “most recent.”

```bash
docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

##### Push an Image

Uploads an image to your Docker Hub account or registry:
```bash
docker image push SOURCE_IMAGE:TAG
```
---

## 🚀 What Happens in docker container run

##### When you run:
```bash
docker container run <image>
```

Docker performs several steps under the hood:

1. Checks for the image locally
2. If missing, pulls it from Docker Hub (or your configured registry)
3. Creates a container from the image
4. Assigns a virtual IP within a private bridge network
5. Maps ports (if specified with -p)
6. Starts the container using the CMD from the image’s Dockerfile

---

##### 🧠 Example Command

```bash
docker container run --publish 80:80 --detach --name test nginx
```

What happens:

1. Downloads the nginx image (if not cached)
2. Creates and starts a new container
3. Opens port 80 on the host → routes to container port 80
4. Runs nginx as a background process (--detach)
5. Names the container test (or random if not specified)

🔹 --detach → Runs container in background

🔹 --name → Assigns a readable name

🔹 NGINX is a lightweight HTTP web server image

---

## 🧾 Logs & Monitoring Commands

 View container logs
```bash
docker container logs <name>
```

Shows live or historical logs from your container (similar to tail -f).

 Show processes inside a container
```bash
docker container top <name>
```

List all containers
```bash
docker container ls -a
```

 Add -a to include stopped containers.
```bash
ps -a
```
> Displays all processes on your host system (useful for cross-checking).

Inspect container configuration
```bash
docker container inspect <name>
```
> Gives full JSON with details like mounts, IPs, environment variables.

View real-time stats
```bash
docker container stats
```
> Similar to top — shows CPU, memory, I/O usage.

Check port mapping
```bash
docker container port <name>
```
Lists which host ports are bound to which container ports.

---

## 🧭 Interactive & Exec Modes

Start and enter a new interactive container
```bash
docker container run -it --name <name> nginx bash
```
- it → Interactive terminal
- bash → Start Bash shell instead of default command
- Exiting the shell stops the container

Restart and attach to a stopped container
```bash
docker container start -ai <name>
```

Execute a command inside a running container
```bash
docker container exec -it <name> bash
```
> Starts a new process (e.g., bash) inside the container — exiting won’t stop it.

---

## 🗑️ Container Cleanup

Remove one or more containers by ID or name:
```bash
docker container rm -f <ID>
```
- -f forces removal of running containers.
- You can remove multiple at once:
```bash
docker container rm -f <id1> <id2> <id3>
```
---

## 🌐 Docker Networking

Key Concepts
- Avoid static IPs — use container names or network aliases.
- Exposed ports (-p or --publish) map host → container ports.

Publish a port
```bash
docker run -p 80:80 nginx
```
> Publishes host port 80 to container port 80.

List networks
```bash
docker network ls
```

Inspect a network
```bash
docker network inspect <network_name>
```

Create a custom network
```bash
docker network create <network_name>
```

Connect a container to a network
```bash
docker network connect <network_name> <container_name>
```

Disconnect a container from a network
```bash
docker network disconnect <network_name> <container_name>
```
---
## 🌉 Network Types

Bridge (default)
- Default virtual network behind the host IP (NAT).
- Used when you --publish ports.
Host
- Attaches container directly to the host’s interface.
- Better performance, less isolation.
None
- No network interface except localhost.

---

## ⚙️ Network Drivers

- The bridge driver is default.
- Third-party drivers extend functionality (e.g., overlay, macvlan, etc.).

---

## 🌐 Docker DNS & Round Robin

Docker daemon includes an internal DNS server.

Containers can resolve each other by name or alias.

Supports DNS Round Robin for load balancing:
multiple containers with the same alias share traffic.

---

## 💾 Docker Volumes

Volumes store data outside of the container lifecycle.
Deleting a container won’t delete its data if it’s on a volume.

List volumes
```bash
docker volume ls
```

Run container with a named volume
```bash
docker container run -d --name mysql \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=True \
  -v mysql-db:/var/lib/mysql mysql
```
- -v mysql-db:/var/lib/mysql → Mounts named volume mysql-db
- Check it with:
```bash
docker volume ls
```

Create a volume
```bash
docker volume create <volume_name>
```
> Useful before docker run if you need custom drivers or labels.

---

📁 Persistent Data — Bind Mounts

Bind mounts map a host directory to a container directory.
Changes reflect instantly in both locations.
- Host files override container files.
- Must be declared at runtime, not in Dockerfile.

Examples:
```bash
# macOS / Linux
docker run -v /Users/parent/stuff:/path/container

# Windows
docker run -v //c/Users/parent/stuff:/path/container

# Using current directory
docker run -v $(pwd):/path/container
```

---

## 🧠 Summary

- Concept -> 	Description
- Image -> 	Blueprint for containers, includes code + dependencies
- Container -> 	Running instance of an image
- Network ->	Virtual communication layer for containers
- Volume ->	Persistent storage separate from containers
- Run ->	Start a container
- Build -> 	Create an image
- Ship -> 	Push/pull images to/from registries

---
