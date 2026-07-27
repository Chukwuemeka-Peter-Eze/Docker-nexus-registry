# Docker Nexus Registry Commands Reference

This document contains the commands used throughout the implementation of **Sonatype Nexus Repository Manager** as a private Docker registry in a local development environment.

The commands are organized according to the deployment workflow so they can be used as a quick reference during future implementations.

---

# Verify Docker Installation

Display the installed Docker version.

```bash
docker --version
```

Display detailed Docker system information.

```bash
docker info
```

List all Docker images stored locally.

```bash
docker images
```

List all running containers.

```bash
docker ps
```

List all containers, including stopped containers.

```bash
docker ps -a
```

---

# Docker Volume Management

Create a persistent volume for Nexus.

```bash
docker volume create nexus-data
```

List all Docker volumes.

```bash
docker volume ls
```

Inspect the Nexus volume.

```bash
docker volume inspect nexus-data
```

Remove the volume when it is no longer required.

```bash
docker volume rm nexus-data
```

---

# Deploy Nexus Repository Manager

Run Nexus as a Docker container.

```bash
docker run -d \
  --name nexus \
  -p 8081:8081 \
  -v nexus-data:/nexus-data \
  sonatype/nexus3
```

Verify that the container is running.

```bash
docker ps
```

Display container details.

```bash
docker inspect nexus
```

Display Nexus logs.

```bash
docker logs nexus
```

Follow logs in real time.

```bash
docker logs -f nexus
```

Restart the Nexus container.

```bash
docker restart nexus
```

Stop the container.

```bash
docker stop nexus
```

Start the container again.

```bash
docker start nexus
```

Remove the container.

```bash
docker rm -f nexus
```

---

# Initial Nexus Configuration

Retrieve the temporary administrator password.

```bash
docker exec nexus cat /nexus-data/admin.password
```

Open an interactive shell inside the container.

```bash
docker exec -it nexus bash
```

If Bash is unavailable, use:

```bash
docker exec -it nexus sh
```

---

# Docker Registry Authentication

Authenticate the Docker client.

```bash
docker login localhost:8083
```

Log out of the registry.

```bash
docker logout localhost:8083
```

---

# Docker Image Management

Display local images.

```bash
docker images
```

Tag an image for the private registry.

```bash
docker tag nginx:latest \
localhost:8083/docker-hosted/nginx:1.0
```

Push the tagged image.

```bash
docker push localhost:8083/docker-hosted/nginx:1.0
```

Pull the image from Nexus.

```bash
docker pull localhost:8083/docker-hosted/nginx:1.0
```

Remove a local image.

```bash
docker rmi localhost:8083/docker-hosted/nginx:1.0
```

---

# Deploy a Container

Run a container using the image stored in Nexus.

```bash
docker run -d \
  --name nginx-private \
  -p 8080:80 \
  localhost:8083/docker-hosted/nginx:1.0
```

View running containers.

```bash
docker ps
```

Stop the container.

```bash
docker stop nginx-private
```

Start the container again.

```bash
docker start nginx-private
```

Remove the container.

```bash
docker rm -f nginx-private
```

---

# Troubleshooting Commands

Display Docker version.

```bash
docker --version
```

Display Docker daemon information.

```bash
docker info
```

Display Nexus logs.

```bash
docker logs nexus
```

Display the last 100 log entries.

```bash
docker logs --tail 100 nexus
```

Inspect the Nexus container.

```bash
docker inspect nexus
```

Display Docker network information.

```bash
docker network ls
```

Display Docker disk usage.

```bash
docker system df
```

---

# Cleanup Commands

Remove stopped containers.

```bash
docker container prune
```

Remove unused images.

```bash
docker image prune
```

Remove unused networks.

```bash
docker network prune
```

Remove unused volumes.

```bash
docker volume prune
```

Remove all unused Docker resources.

```bash
docker system prune
```

Remove everything, including unused volumes.

```bash
docker system prune -a --volumes
```

---

# Useful URLs

Nexus Repository Manager

```text
http://localhost:8081
```

Docker Hosted Repository

```text
localhost:8083
```

Sample Application

```text
http://localhost:8080
```

---

# Command Summary

| Task               | Primary Command                                    |
| ------------------ | -------------------------------------------------- |
| Verify Docker      | `docker info`                                      |
| Create Volume      | `docker volume create nexus-data`                  |
| Deploy Nexus       | `docker run ...`                                   |
| View Logs          | `docker logs nexus`                                |
| Get Admin Password | `docker exec nexus cat /nexus-data/admin.password` |
| Login to Registry  | `docker login localhost:8083`                      |
| Tag Image          | `docker tag ...`                                   |
| Push Image         | `docker push ...`                                  |
| Pull Image         | `docker pull ...`                                  |
| Run Container      | `docker run ...`                                   |
| Cleanup            | `docker system prune`                              |

