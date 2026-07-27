# Nexus Repository Manager Setup Guide

This guide walks through the complete process of deploying **Sonatype Nexus Repository Manager** as a private Docker registry in a local development environment.

By the end of this guide, you will have:

* A running Nexus Repository Manager instance
* A configured Docker Hosted Repository
* Docker client authentication
* The ability to push and pull Docker images
* A working private container registry

---

# Overview

Nexus Repository Manager is a universal artifact repository that stores and manages software packages and container images.

In this project, Nexus is deployed as a Docker container and configured to act as a private Docker registry.

The implementation uses a local Docker environment, making it suitable for learning container registry workflows without requiring cloud infrastructure.

---

# Prerequisites

Ensure the following software is installed before beginning.

| Requirement   | Purpose              |
| ------------- | -------------------- |
| Docker Engine | Container runtime    |
| Docker CLI    | Docker management    |
| Git           | Version control      |
| Web Browser   | Nexus administration |
| Terminal      | Execute commands     |

Verify Docker installation.

```bash
docker --version
docker info
```

---

# Step 1 — Create Persistent Storage

Create a Docker volume to store Nexus configuration and repository data.

```bash
docker volume create nexus-data
```

Verify that the volume exists.

```bash
docker volume ls
```

Using a persistent volume ensures that uploaded artifacts and configuration remain available even if the Nexus container is recreated.

---

# Step 2 — Deploy Nexus Repository Manager

Start Nexus as a Docker container.

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

The first startup may take several minutes because Nexus initializes its internal components.

---

# Step 3 — Access the Web Interface

Open your browser.

Navigate to:

```text
http://localhost:8081
```

Wait until the login page appears.

---

# Step 4 — Retrieve the Initial Password

Retrieve the temporary administrator password.

```bash
docker exec nexus cat /nexus-data/admin.password
```

Login using:

Username

```text
admin
```

Password

```text
<output of admin.password>
```

Create a new administrator password when prompted.

---

# Step 5 — Complete Initial Configuration

Complete the setup wizard.

Typical tasks include:

* Change administrator password
* Configure anonymous access
* Finish initialization

After completing the wizard, the Nexus dashboard becomes available.

---

# Step 6 — Create a Docker Hosted Repository

Navigate through the Nexus interface.

```text
Repositories
      ↓
Create Repository
      ↓
Docker (Hosted)
```

Configure the repository.

| Setting         | Value         |
| --------------- | ------------- |
| Repository Name | docker-hosted |
| HTTP Connector  | Enabled       |
| HTTP Port       | 8083          |
| Blob Store      | Default       |

Save the repository.

---

# Step 7 — Authenticate Docker

Authenticate the Docker client.

```bash
docker login localhost:8083
```

Use your Nexus administrator credentials.

Successful authentication returns:

```text
Login Succeeded
```

---

# Step 8 — Tag a Docker Image

Display local images.

```bash
docker images
```

Tag an image.

```bash
docker tag nginx:latest \
localhost:8083/docker-hosted/nginx:1.0
```

Verify the new tag.

```bash
docker images
```

---

# Step 9 — Push the Image

Publish the image.

```bash
docker push localhost:8083/docker-hosted/nginx:1.0
```

Wait for Docker to upload all image layers.

After completion, verify the image in the Nexus web interface under the **docker-hosted** repository.

---

# Step 10 — Pull the Image

Retrieve the image from the private registry.

```bash
docker pull localhost:8083/docker-hosted/nginx:1.0
```

Successful completion confirms that the registry is functioning correctly.

---

# Step 11 — Deploy a Container

Run a container using the image stored in Nexus.

```bash
docker run -d \
  --name nginx-private \
  -p 8080:80 \
  localhost:8083/docker-hosted/nginx:1.0
```

Verify that the container is running.

```bash
docker ps
```

Open a browser and navigate to:

```text
http://localhost:8080
```

If the Nginx welcome page is displayed, the deployment was successful.

---

# Validation Checklist

Confirm that the following tasks have been completed.

* Docker installed successfully
* Nexus container running
* Nexus web interface accessible
* Administrator account configured
* Docker Hosted Repository created
* Docker client authenticated
* Image tagged
* Image pushed successfully
* Image visible in Nexus
* Image pulled successfully
* Container deployed successfully

---

# Common Issues

| Issue                           | Possible Solution                                                       |
| ------------------------------- | ----------------------------------------------------------------------- |
| Nexus startup is slow           | Wait several minutes for initialization to complete.                    |
| Unable to access localhost:8081 | Verify the Nexus container is running and the port mapping is correct.  |
| Docker login fails              | Confirm the repository port and credentials.                            |
| Push denied                     | Check repository permissions and image tag.                             |
| Image not visible               | Refresh the Nexus interface and verify the push completed successfully. |

---

# Conclusion

You have successfully deployed Sonatype Nexus Repository Manager as a private Docker registry in a local Docker environment.

The registry can now be used to store, version, and distribute Docker images securely. The same workflow can be adapted for larger environments, integrated into CI/CD pipelines, or extended to support additional artifact formats managed by Nexus Repository Manager.
