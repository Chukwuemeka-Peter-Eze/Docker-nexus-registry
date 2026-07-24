# Setup Guide

This guide provides a step-by-step procedure for deploying **Nexus Repository Manager** as a private Docker registry on an AWS EC2 instance.

The objective is to configure a secure private registry that can store, manage, and distribute Docker images within an organization.

---

# Table of Contents

- Prerequisites
- AWS Infrastructure
- Install Docker
- Deploy Nexus Repository Manager
- Access Nexus
- Configure Initial Setup
- Create Docker Hosted Repository
- Configure Docker Authentication
- Push Docker Images
- Pull Docker Images
- Verify Deployment
- Cleanup

---

# Prerequisites

Before beginning, ensure you have the following:

## AWS

- AWS Account
- Ubuntu EC2 Instance
- Public IP Address
- Security Group
- SSH Access

---

## Local Machine

- Docker CLI
- SSH Client
- Git
- Internet Connection

---

# Step 1 — Connect to AWS

SSH into the EC2 instance.

```bash
ssh -i your-key.pem ubuntu@<EC2-Public-IP>
```

Verify connectivity before continuing.

---

# Step 2 — Update Ubuntu

```bash
sudo apt update

sudo apt upgrade -y
```

Keeping the operating system updated helps ensure compatibility and security.

---

# Step 3 — Verify Docker

Verify Docker is installed.

```bash
docker --version
```

Verify Docker daemon status.

```bash
docker info
```

---

# Step 4 — Create Persistent Storage

Create a Docker volume for Nexus.

```bash
docker volume create nexus-data
```

Verify the volume.

```bash
docker volume ls
```

---

# Step 5 — Deploy Nexus Repository Manager

Run Nexus inside a Docker container.

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

The initial startup may take several minutes.

---

# Step 6 — Access Nexus

Open your browser.

```text
http://<EC2-Public-IP>:8081
```

Wait until the login page becomes available.

---

# Step 7 — Retrieve the Initial Password

Retrieve the administrator password stored inside the container.

```bash
docker exec nexus \
cat /nexus-data/admin.password
```

Login using:

Username

```text
admin
```

Password

```text
<admin.password>
```

After signing in, create a new administrator password.

---

# Step 8 — Complete Initial Configuration

During the first login:

- Change the administrator password.
- Configure anonymous access according to your requirements.
- Complete the initialization wizard.

The Nexus dashboard should now be available.

---

# Step 9 — Create a Docker Hosted Repository

Navigate to:

```text
Repositories

↓

Create Repository

↓

Docker (Hosted)
```

Configure the repository.

Example settings:

| Setting | Value |
|----------|-------|
| Repository Name | docker-hosted |
| Repository Type | Hosted |
| Blob Store | Default |
| Deployment Policy | Allow Redeploy (or your preferred policy) |
| Docker Connector | Configure the required port |

Save the repository.

---

# Step 10 — Configure Docker Authentication

Authenticate the Docker client.

```bash
docker login <EC2-Public-IP>:<Docker-Repository-Port>
```

Example:

```bash
docker login 54.xxx.xxx.xxx:8083
```

Successful authentication returns:

```text
Login Succeeded
```

---

# Step 11 — Verify Local Images

Display locally available Docker images.

```bash
docker images
```

Choose an image to upload to the private registry.

---

# Step 12 — Tag the Image

Tag the image using the Nexus registry endpoint.

Example:

```bash
docker tag nginx:latest \
<EC2-Public-IP>:<Docker-Repository-Port>/docker-hosted/nginx:1.0
```

Verify the new tag.

```bash
docker images
```

---

# Step 13 — Push the Image

Upload the image.

```bash
docker push \
<EC2-Public-IP>:<Docker-Repository-Port>/docker-hosted/nginx:1.0
```

Docker uploads every image layer to Nexus.

---

# Step 14 — Verify the Image

Open the Nexus dashboard.

Navigate to:

```text
Browse

↓

docker-hosted
```

Confirm that the image appears in the repository.

---

# Step 15 — Pull the Image

Download the image from the private registry.

```bash
docker pull \
<EC2-Public-IP>:<Docker-Repository-Port>/docker-hosted/nginx:1.0
```

This verifies successful retrieval.

---

# Step 16 — Deploy a Container

Run a container from the image stored in Nexus.

```bash
docker run -d \
--name nginx-private \
-p 8080:80 \
<EC2-Public-IP>:<Docker-Repository-Port>/docker-hosted/nginx:1.0
```

Verify deployment.

```bash
docker ps
```

---

# Step 17 — Validate the Deployment

Confirm:

- Nexus is running.
- Docker Hosted Repository is available.
- Docker login succeeded.
- Image uploaded successfully.
- Image appears in Nexus.
- Image pulled successfully.
- Container started successfully.

---

# Cleanup

Stop Nexus.

```bash
docker stop nexus
```

Remove the container.

```bash
docker rm nexus
```

Remove unused resources.

```bash
docker system prune
```

Remove the Docker volume only if the repository is no longer needed.

```bash
docker volume rm nexus-data
```

---

# Deployment Workflow

```text
Launch EC2
      │
      ▼
Install Docker
      │
      ▼
Create Docker Volume
      │
      ▼
Deploy Nexus
      │
      ▼
Login to Nexus
      │
      ▼
Create Docker Repository
      │
      ▼
Docker Login
      │
      ▼
Tag Image
      │
      ▼
Push Image
      │
      ▼
Store Image
      │
      ▼
Pull Image
      │
      ▼
Run Container
      │
      ▼
Verify Deployment
```

---

# Conclusion

Following this guide results in a fully functional private Docker registry hosted on AWS using Nexus Repository Manager. The completed environment enables secure image storage, controlled access, centralized image management, and reliable image distribution, providing a strong foundation for modern DevOps and CI/CD workflows.
