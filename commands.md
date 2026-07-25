# Docker Nexus Registry Commands Reference

This document contains the primary Docker and Nexus Repository Manager commands used throughout this project. The commands are organized according to the deployment workflow, making them easy to follow and reproduce.

---

# Table of Contents

- Verify Docker Installation
- Create Persistent Storage
- Deploy Nexus Repository Manager
- Verify Nexus Container
- Retrieve Administrator Password
- Configure Docker Authentication
- Verify Local Images
- Tag Docker Images
- Push Images to Nexus
- Pull Images from Nexus
- Run Containers from Private Images
- Inspect Docker Resources
- Cleanup Commands
- Complete Deployment Workflow
- Command Summary

---

# Verify Docker Installation

Display the installed Docker version.

```bash
docker --version
```

Display Docker system information.

```bash
docker info
```

These commands confirm that Docker is installed and the Docker daemon is running.

---

# Create Persistent Storage

Create a Docker volume for Nexus Repository Manager.

```bash
docker volume create nexus-data
```

Purpose:

- Stores Nexus configuration.
- Persists uploaded Docker images.
- Prevents data loss during container recreation.

Verify the volume.

```bash
docker volume ls
```

Inspect the volume.

```bash
docker volume inspect nexus-data
```

---

# Deploy Nexus Repository Manager

Start Nexus as a Docker container.

```bash
docker run -d \
--name nexus \
-p 8081:8081 \
-v nexus-data:/nexus-data \
sonatype/nexus3
```

This command:

- Creates the Nexus container.
- Maps the web interface to port 8081.
- Mounts persistent storage.
- Runs the container in detached mode.

---

# Verify the Nexus Container

Display running containers.

```bash
docker ps
```

Display all containers.

```bash
docker ps -a
```

View container logs.

```bash
docker logs nexus
```

These commands help verify that Nexus has started successfully.

---

# Retrieve the Initial Administrator Password

Obtain the default administrator password.

```bash
docker exec nexus \
cat /nexus-data/admin.password
```

Use this password during the initial login to the Nexus web interface.

---

# Docker Client Authentication

Authenticate the Docker client with the private registry.

```bash
docker login <EC2-Public-IP>:<Registry-Port>
```

Example:

```bash
docker login 54.xxx.xxx.xxx:8083
```

After entering the username and password, Docker should return:

```text
Login Succeeded
```

---

# Display Local Docker Images

List locally available images.

```bash
docker images
```

Review the image that will be uploaded to Nexus.

---

# Tag a Docker Image

Before an image can be pushed to a private registry, it must be tagged using the registry endpoint.

Example:

```bash
docker tag nginx:latest \
<EC2-Public-IP>:<Registry-Port>/docker-hosted/nginx:1.0
```

Example:

```bash
docker tag nginx:latest \
54.xxx.xxx.xxx:8083/docker-hosted/nginx:1.0
```

Verify the new tag.

```bash
docker images
```

---

# Push an Image to Nexus

Upload the tagged image.

```bash
docker push \
<EC2-Public-IP>:<Registry-Port>/docker-hosted/nginx:1.0
```

Docker uploads each image layer individually.

Successful completion confirms that the image has been stored in the private registry.

---

# Pull an Image from Nexus

Download an image from the private registry.

```bash
docker pull \
<EC2-Public-IP>:<Registry-Port>/docker-hosted/nginx:1.0
```

This verifies that the image can be retrieved successfully.

---

# Run a Container from the Private Registry

Deploy a container using the image stored in Nexus.

```bash
docker run -d \
--name nginx-private \
-p 8080:80 \
<EC2-Public-IP>:<Registry-Port>/docker-hosted/nginx:1.0
```

Verify the running container.

```bash
docker ps
```

---

# Inspect Docker Volumes

Display Docker volumes.

```bash
docker volume ls
```

Inspect the Nexus volume.

```bash
docker volume inspect nexus-data
```

This confirms that repository data is stored persistently.

---

# Inspect Docker Networks

Display Docker networks.

```bash
docker network ls
```

Inspect a specific network.

```bash
docker network inspect bridge
```

Useful when troubleshooting container connectivity.

---

# Display Docker Resource Usage

Display Docker storage usage.

```bash
docker system df
```

Review:

- Images
- Containers
- Volumes
- Build cache

---

# View Docker Events

Monitor Docker events in real time.

```bash
docker events
```

Useful for observing container lifecycle events during deployment.

---

# Cleanup Commands

Stop the Nexus container.

```bash
docker stop nexus
```

Remove the container.

```bash
docker rm nexus
```

Remove unused containers.

```bash
docker container prune
```

Remove unused images.

```bash
docker image prune
```

Remove unused Docker resources.

```bash
docker system prune
```

Remove all unused resources.

```bash
docker system prune -a
```

Remove the Nexus volume (only if the repository is no longer needed).

```bash
docker volume rm nexus-data
```

---

# Complete Deployment Workflow

```bash
docker volume create nexus-data

docker run -d \
--name nexus \
-p 8081:8081 \
-v nexus-data:/nexus-data \
sonatype/nexus3

docker ps

docker logs nexus

docker exec nexus \
cat /nexus-data/admin.password

docker login <EC2-Public-IP>:<Registry-Port>

docker images

docker tag nginx:latest \
<EC2-Public-IP>:<Registry-Port>/docker-hosted/nginx:1.0

docker push \
<EC2-Public-IP>:<Registry-Port>/docker-hosted/nginx:1.0

docker pull \
<EC2-Public-IP>:<Registry-Port>/docker-hosted/nginx:1.0

docker run -d \
--name nginx-private \
-p 8080:80 \
<EC2-Public-IP>:<Registry-Port>/docker-hosted/nginx:1.0

docker ps

docker system df
```

This sequence represents the complete implementation workflow from deploying Nexus to publishing and deploying a private Docker image.

---

# Command Summary

| Command | Purpose |
|----------|---------|
| `docker volume create` | Create persistent storage |
| `docker volume ls` | List Docker volumes |
| `docker volume inspect` | Inspect a Docker volume |
| `docker run` | Create and start a container |
| `docker ps` | List running containers |
| `docker logs` | View container logs |
| `docker exec` | Execute commands inside a container |
| `docker login` | Authenticate with a registry |
| `docker images` | List local images |
| `docker tag` | Tag an image for a registry |
| `docker push` | Upload an image to Nexus |
| `docker pull` | Download an image from Nexus |
| `docker network ls` | List Docker networks |
| `docker network inspect` | Inspect a Docker network |
| `docker system df` | Display Docker storage usage |
| `docker events` | Monitor Docker activity |
| `docker container prune` | Remove stopped containers |
| `docker image prune` | Remove unused images |
| `docker system prune` | Remove unused Docker resources |
| `docker volume rm` | Remove a Docker volume |

---

# Conclusion

These commands represent the complete operational workflow for deploying Nexus Repository Manager as a private Docker registry, authenticating Docker clients, publishing images, retrieving images, and maintaining the registry environment. Together, they provide a practical reference for implementing secure private container image management on AWS.
