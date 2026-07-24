# Docker Nexus Repository as a Private Docker Registry on AWS

<p align="center">

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Nexus Repository](https://img.shields.io/badge/Nexus_Repository_Manager-4CAF50?style=for-the-badge)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![DevOps](https://img.shields.io/badge/DevOps-0A66C2?style=for-the-badge)

</p>

---

# Project Overview

This project demonstrates how to deploy and configure **Sonatype Nexus Repository Manager** as a **private Docker registry** on an AWS EC2 instance. The implementation covers the complete lifecycle of private container image management, from installing Nexus and configuring a Docker hosted repository to authenticating Docker clients, pushing images, pulling images, and validating deployments.

While public registries such as Docker Hub are suitable for distributing public images, many organizations require a secure, centralized repository to store proprietary container images, manage versions, control access, and improve software supply chain security. Nexus Repository Manager addresses these requirements by acting as a private artifact repository for Docker images and many other package formats.

This project was implemented on an Ubuntu-based AWS EC2 instance using Docker Engine. Throughout the implementation, I documented the setup process, registry configuration, authentication workflow, image publishing process, troubleshooting techniques, architecture, and operational best practices.

---

# Project Objectives

The primary objectives of this project were to:

- Deploy Nexus Repository Manager on AWS.
- Configure Nexus as a private Docker registry.
- Create and configure a Docker hosted repository.
- Configure Docker client authentication.
- Tag Docker images correctly for a private registry.
- Push Docker images to Nexus.
- Pull images from the private registry.
- Verify successful image deployment.
- Understand private registry workflows.
- Document the complete implementation for reproducibility.

---

# Why Use a Private Docker Registry?

Container images are valuable software artifacts. In enterprise environments, storing them in a public registry is often inappropriate due to security, compliance, and intellectual property requirements.

A private Docker registry enables organizations to:

- Store proprietary application images securely.
- Control who can publish and pull images.
- Maintain internal image versions.
- Improve software supply chain security.
- Reduce dependency on public registries.
- Centralize image management.
- Support development, testing, and production workflows consistently.

Nexus Repository Manager provides these capabilities while supporting multiple artifact types beyond Docker images.

---

# What is Nexus Repository Manager?

Nexus Repository Manager is an artifact repository platform that stores, manages, and distributes software packages and container images.

In this project, Nexus is configured as a **private Docker registry**, allowing Docker clients to authenticate, publish images, and retrieve images from a controlled internal repository.

Although this implementation focuses on Docker, Nexus also supports repositories for technologies such as Maven, npm, NuGet, PyPI, Helm, and others, making it a common component in enterprise CI/CD pipelines.

---

# Public Registry vs Private Registry

| Feature | Public Registry | Private Registry (Nexus) |
|----------|-----------------|---------------------------|
| Access | Public | Controlled |
| Authentication | Optional | Required |
| Proprietary Images | Not Recommended | Supported |
| Version Control | Yes | Yes |
| Access Management | Limited | Granular |
| Enterprise Ready | Limited | Yes |
| Internal Software Distribution | Limited | Yes |

---

# Project Architecture

```text
                     Developer
                          │
                          ▼
                  GitHub Repository
                          │
                          ▼
                    AWS EC2 Instance
                          │
                          ▼
                     Docker Engine
                          │
                          ▼
             Nexus Repository Manager
                          │
                Docker Hosted Repository
                    ┌──────────────┐
                    │              │
              Docker Push     Docker Pull
                    │              │
                    └──────┬───────┘
                           ▼
                   Docker Images
                           │
                           ▼
                Running Containers
```

This architecture demonstrates how Nexus Repository Manager acts as the centralized repository for storing, distributing, and managing Docker images within an AWS-hosted environment.

---

# Technologies Used

| Technology | Purpose |
|------------|---------|
| AWS EC2 | Cloud Compute |
| Ubuntu Linux | Operating System |
| Docker Engine | Container Runtime |
| Nexus Repository Manager | Private Docker Registry |
| Docker CLI | Image Management |
| Git | Version Control |
| GitHub | Repository Hosting |
| SSH | Remote Administration |

---

# Key Skills Demonstrated

This project demonstrates practical experience with:

- Private Docker registries
- Nexus Repository Manager
- Docker image tagging
- Docker authentication
- Image publishing
- Image retrieval
- Docker registry administration
- Software artifact management
- AWS infrastructure
- Linux server administration
- DevOps documentation
- Operational troubleshooting

---

# Architecture Diagram

> Replace the placeholder below after creating the Draw.io architecture diagram.

```text
images/architecture.png
```

<p align="center">

![Nexus Architecture](images/architecture.png)

</p>

---

# High-Level Workflow

```text
Create Docker Image
        │
        ▼
Tag Image
        │
        ▼
Authenticate Docker Client
        │
        ▼
Push Image to Nexus
        │
        ▼
Store Image in Private Registry
        │
        ▼
Pull Image
        │
        ▼
Run Container
        │
        ▼
Verify Successful Deployment
```

This workflow demonstrates the complete lifecycle of managing Docker images using a private registry hosted on AWS.

---

# Repository Structure

```text
Docker-nexus-registry/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── architecture/
│   └── architecture.drawio
│
├── docs/
│   ├── setup.md
│   ├── troubleshooting.md
│   └── lessons-learned.md
│
├── images/
│   ├── architecture.png
│   ├── ec2-instance.png
│   ├── docker-installation.png
│   ├── nexus-running.png
│   ├── nexus-login.png
│   ├── docker-hosted-repository.png
│   ├── docker-login.png
│   ├── docker-tag.png
│   ├── docker-push.png
│   ├── docker-pull.png
│   ├── repository-images.png
│   ├── container-running.png
│   └── cleanup.png
│
├── commands.md
└── video-script.md
```

---

# Learning Outcomes

By completing this project, I gained practical experience in:

- Deploying Nexus Repository Manager on AWS.
- Configuring a private Docker registry.
- Managing Docker hosted repositories.
- Authenticating Docker clients.
- Publishing Docker images.
- Retrieving Docker images from a private registry.
- Understanding secure image management workflows.
- Documenting infrastructure implementations.
- Applying software supply chain best practices.

---

---

# AWS Infrastructure

The entire implementation was performed on **Amazon Web Services (AWS)** using an Ubuntu EC2 instance.

Hosting Nexus Repository Manager on AWS provides a practical environment for learning enterprise container image management and simulates how private registries are commonly deployed in production.

---

## Infrastructure Components

| Component | Purpose |
|-----------|---------|
| Cloud Provider | Amazon Web Services (AWS) |
| Compute Service | Amazon EC2 |
| Operating System | Ubuntu Linux |
| Container Runtime | Docker Engine |
| Private Registry | Nexus Repository Manager |
| Version Control | Git |
| Repository Hosting | GitHub |
| Remote Access | SSH |

---

# Prerequisites

Before beginning this implementation, ensure the following resources are available.

## Cloud Resources

- AWS Account
- Amazon EC2 Instance
- Public IP Address
- Security Group configured
- Internet Connectivity

## Software Requirements

- Ubuntu Linux
- Docker Engine
- Docker CLI
- Git
- SSH Client
- Web Browser

---

# Solution Overview

This project follows the complete lifecycle of private container image management.

```text
Developer
      │
      ▼
AWS EC2 Instance
      │
      ▼
Docker Engine
      │
      ▼
Run Nexus Repository
      │
      ▼
Create Docker Hosted Repository
      │
      ▼
Configure Docker Client
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
Deploy Container
```

---

# Step 1 — Launch an EC2 Instance

Launch an Ubuntu EC2 instance.

Recommended configuration:

- Ubuntu Server LTS
- Public IP Enabled
- SSH Enabled
- Minimum 4 GB RAM
- At least 2 vCPUs
- Adequate storage for Docker images

Nexus Repository Manager benefits from additional memory, especially when managing multiple repositories.

---

## Screenshot Placeholder

```text
images/ec2-instance.png
```

<p align="center">

![EC2 Instance](images/ec2-instance.png)

</p>

---

# Step 2 — Configure Security Groups

Ensure the EC2 security group allows the required inbound traffic.

| Port | Purpose |
|------|----------|
| 22 | SSH |
| 8081 | Nexus Web Interface |
| Docker Registry Port | Docker Hosted Repository |

The Docker registry port depends on how the hosted repository is configured.

---

# Step 3 — Connect to the Server

Connect using SSH.

```bash
ssh -i your-key.pem ubuntu@<EC2-Public-IP>
```

After connecting, verify that the server is accessible before proceeding.

---

# Step 4 — Update Ubuntu

Update installed packages.

```bash
sudo apt update

sudo apt upgrade -y
```

Keeping the operating system updated improves security and compatibility.

---

# Step 5 — Verify Docker Installation

Confirm Docker is installed correctly.

```bash
docker --version
```

Display Docker system information.

```bash
docker info
```

Ensure that the Docker daemon is running successfully before deploying Nexus.

---

## Screenshot Placeholder

```text
images/docker-installation.png
```

<p align="center">

![Docker Installation](images/docker-installation.png)

</p>

---

# Why Run Nexus in Docker?

Running Nexus inside a Docker container provides several operational benefits.

- Simplified deployment
- Consistent runtime environment
- Easy upgrades
- Container isolation
- Portable configuration
- Faster recovery

Containerizing infrastructure services is a common DevOps practice that improves repeatability and maintainability.

---

# Understanding Nexus Repository Manager

Nexus Repository Manager serves as a centralized repository for software artifacts.

For this project, Nexus functions as a **private Docker registry**, allowing authenticated Docker clients to publish and retrieve container images securely.

The repository becomes the trusted source for internally managed Docker images.

---

# Nexus Architecture

```text
Docker Client
      │
      ▼
Docker Login
      │
      ▼
Nexus Repository Manager
      │
      ▼
Docker Hosted Repository
      │
      ▼
Docker Images
      │
      ▼
Application Deployment
```

---

# Container Image Workflow

This project demonstrates the following image lifecycle.

```text
Build Docker Image
        │
        ▼
Tag Image
        │
        ▼
Authenticate
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
```

This workflow reflects how container images are commonly managed within enterprise environments.

---

# Benefits of a Private Registry

Using Nexus Repository Manager provides several advantages over relying exclusively on public registries.

- Secure internal image storage
- Controlled access
- Image version management
- Centralized repository
- Reduced external dependencies
- Better software supply chain governance
- Support for enterprise CI/CD workflows

---

# Preparing for Nexus Deployment

Before starting Nexus, verify:

- Docker Engine is running.
- Required ports are open.
- EC2 instance has sufficient resources.
- Internet connectivity is available.
- SSH access is functioning.
- Docker CLI is operational.

Completing these checks reduces the likelihood of deployment issues later in the process.

---

# Screenshot Gallery

Replace each placeholder with the corresponding screenshots from your implementation.

| Activity | Screenshot |
|----------|------------|
| EC2 Instance Running | `images/ec2-instance.png` |
| SSH Connection | `images/ssh-connection.png` *(optional)* |
| Ubuntu Updated | `images/system-update.png` *(optional)* |
| Docker Installation | `images/docker-installation.png` |

---

---

# Deploying Nexus Repository Manager

Nexus Repository Manager is deployed as a Docker container running on the AWS EC2 instance.

Running Nexus inside Docker simplifies installation, improves portability, and aligns with modern infrastructure management practices.

Once deployed, Nexus becomes the organization's private repository for Docker images and other software artifacts.

---

# Step 6 — Create a Docker Volume for Nexus

Before deploying Nexus, create a Docker volume to persist repository data.

```bash
docker volume create nexus-data
```

Using a named Docker volume ensures that repository data survives container restarts, upgrades, and recreation.

Verify the volume.

```bash
docker volume ls
```

---

## Screenshot Placeholder

```text
images/nexus-volume.png
```

<p align="center">

![Docker Volume for Nexus](images/nexus-volume.png)

</p>

---

# Step 7 — Deploy Nexus Repository Manager

Start Nexus as a Docker container.

Example:

```bash
docker run -d \
--name nexus \
-p 8081:8081 \
-v nexus-data:/nexus-data \
sonatype/nexus3
```

This command performs the following:

- Creates a Nexus container
- Maps port **8081** to the host
- Mounts persistent storage
- Starts Nexus in detached mode

Verify the container.

```bash
docker ps
```

---

## Screenshot Placeholder

```text
images/nexus-running.png
```

<p align="center">

![Running Nexus Container](images/nexus-running.png)

</p>

---

# Understanding the Deployment

```text
AWS EC2
     │
     ▼
Docker Engine
     │
     ▼
Nexus Container
     │
     ▼
Docker Volume

nexus-data
```

All repository configuration and uploaded Docker images are stored inside the persistent volume.

This prevents data loss if the Nexus container is recreated.

---

# Step 8 — Access the Nexus Web Interface

Open a browser.

Navigate to:

```text
http://<EC2-Public-IP>:8081
```

The Nexus welcome page should appear.

The first startup may take several minutes because Nexus initializes its internal components.

---

## Screenshot Placeholder

```text
images/nexus-login.png
```

<p align="center">

![Nexus Login](images/nexus-login.png)

</p>

---

# Step 9 — Retrieve the Initial Administrator Password

The initial administrator password is stored inside the Nexus container.

Retrieve it using:

```bash
docker exec nexus \
cat /nexus-data/admin.password
```

Copy the password.

Log into Nexus using:

Username

```text
admin
```

Password

```text
<admin.password>
```

After signing in, Nexus prompts you to change the password.

---

## Screenshot Placeholder

```text
images/admin-password.png
```

<p align="center">

![Initial Admin Password](images/admin-password.png)

</p>

---

# Step 10 — Complete Initial Configuration

During first login:

- Change the administrator password.
- Configure anonymous access according to your requirements.
- Complete the setup wizard.

After completing the wizard, the Nexus dashboard becomes available.

---

## Screenshot Placeholder

```text
images/nexus-dashboard.png
```

<p align="center">

![Nexus Dashboard](images/nexus-dashboard.png)

</p>

---

# Understanding Nexus Components

The Nexus dashboard provides centralized management for software repositories.

Major components include:

- Repositories
- Security
- Users
- Roles
- Blob Stores
- Tasks
- System Information

For this project, the primary focus is the **Docker Hosted Repository**.

---

# Step 11 — Create a Docker Hosted Repository

Navigate to:

```text
Repositories
```

↓

```text
Create Repository
```

↓

```text
Docker (Hosted)
```

Configure the repository.

Typical settings include:

| Setting | Description |
|----------|-------------|
| Repository Name | docker-hosted |
| Repository Type | Hosted |
| Deployment Policy | Allow Redeploy (or as required) |
| Blob Store | Default |
| HTTP Port | Configure as required |

Save the repository.

---

## Screenshot Placeholder

```text
images/docker-hosted-repository.png
```

<p align="center">

![Docker Hosted Repository](images/docker-hosted-repository.png)

</p>

---

# Understanding the Docker Hosted Repository

The hosted repository stores internally managed Docker images.

Typical workflow:

```text
Docker Client

        │

docker login

        │

docker push

        │

Docker Hosted Repository

        │

docker pull

        │

Application Deployment
```

Instead of downloading images from a public registry, internal teams retrieve approved images from Nexus.

---

# Verify Repository Creation

After creating the repository, confirm:

- Repository is listed.
- Repository is online.
- Docker connector is configured.
- Repository status is healthy.

At this point, Nexus is ready to accept Docker client connections.

---

# Screenshot Gallery

Replace each placeholder with your implementation screenshots.

| Activity | Screenshot |
|----------|------------|
| Docker Volume Created | `images/nexus-volume.png` |
| Nexus Container Running | `images/nexus-running.png` |
| Nexus Login Page | `images/nexus-login.png` |
| Initial Password Retrieval | `images/admin-password.png` |
| Nexus Dashboard | `images/nexus-dashboard.png` |
| Docker Hosted Repository | `images/docker-hosted-repository.png` |

---

# Deployment Summary

The following milestones have now been completed:

- Docker volume created for persistent Nexus storage.
- Nexus Repository Manager deployed in Docker.
- Persistent storage configured.
- Nexus web interface accessed.
- Initial administrator password retrieved.
- Administrator account configured.
- Docker Hosted Repository created.
- Repository verified and ready for Docker client authentication.

---

---

# Configuring Docker Authentication

After creating the Docker Hosted Repository, the next step is to configure the Docker client so it can securely communicate with Nexus Repository Manager.

Authentication ensures that only authorized users can publish and retrieve container images from the private registry.

---

# Understanding the Authentication Workflow

```text
Docker Client
      │
      ▼
Docker Login
      │
      ▼
Nexus Repository Manager
      │
      ▼
Authentication Successful
      │
      ▼
Push Images
      │
      ▼
Pull Images
```

This authentication process protects private images from unauthorized access.

---

# Step 12 — Configure the Docker Client

Docker communicates with registries using the Docker CLI.

Before pushing or pulling images, authenticate against the Nexus Docker Hosted Repository.

Run:

```bash
docker login <EC2-Public-IP>:<Docker-Repository-Port>
```

Example:

```bash
docker login 54.xxx.xxx.xxx:8083
```

> **Note:** Replace the IP address and Docker repository port with the values configured in your AWS environment.

Enter:

Username

```text
admin
```

Password

```text
Your Nexus Password
```

If authentication succeeds, Docker returns:

```text
Login Succeeded
```

---

## Screenshot Placeholder

```text
images/docker-login.png
```

<p align="center">

![Docker Login](images/docker-login.png)

</p>

---

# Step 13 — Verify Existing Images

Display locally available images.

```bash
docker images
```

Example output:

```text
REPOSITORY          TAG       IMAGE ID
nginx               latest    xxxxxxxxx
node                latest    xxxxxxxxx
ubuntu              latest    xxxxxxxxx
```

Choose the image that will be published to the private registry.

---

## Screenshot Placeholder

```text
images/docker-images.png
```

<p align="center">

![Local Docker Images](images/docker-images.png)

</p>

---

# Understanding Image Tagging

Docker identifies images using tags.

A private registry requires images to be tagged with the registry endpoint before they can be pushed.

General format:

```text
<Registry>/<Repository>/<Image>:<Tag>
```

Example:

```text
54.xxx.xxx.xxx:8083/docker-hosted/nginx:1.0
```

---

# Step 14 — Tag the Docker Image

Tag the image for the Nexus repository.

Example:

```bash
docker tag nginx:latest \
<EC2-Public-IP>:<Docker-Repository-Port>/docker-hosted/nginx:1.0
```

Example:

```bash
docker tag nginx:latest \
54.xxx.xxx.xxx:8083/docker-hosted/nginx:1.0
```

Display images again.

```bash
docker images
```

The tagged image should now appear.

---

## Screenshot Placeholder

```text
images/docker-tag.png
```

<p align="center">

![Docker Image Tagging](images/docker-tag.png)

</p>

---

# Step 15 — Push the Image to Nexus

Upload the image to the private registry.

```bash
docker push \
<EC2-Public-IP>:<Docker-Repository-Port>/docker-hosted/nginx:1.0
```

Docker uploads each image layer.

Typical workflow:

```text
Preparing

Waiting

Pushing Layers

Image Uploaded

Push Complete
```

---

## Screenshot Placeholder

```text
images/docker-push.png
```

<p align="center">

![Docker Push](images/docker-push.png)

</p>

---

# Verify the Uploaded Image

Return to the Nexus web interface.

Navigate to:

```text
Browse
```

↓

```text
docker-hosted
```

The uploaded image should now be visible.

Information typically displayed includes:

- Repository
- Image name
- Tag
- Digest
- Upload timestamp

---

## Screenshot Placeholder

```text
images/repository-images.png
```

<p align="center">

![Images Stored in Nexus](images/repository-images.png)

</p>

---

# Step 16 — Pull an Image from Nexus

Verify that the image can be retrieved successfully.

```bash
docker pull \
<EC2-Public-IP>:<Docker-Repository-Port>/docker-hosted/nginx:1.0
```

Docker downloads the image layers from the private registry.

Successful completion confirms that the registry is functioning correctly.

---

## Screenshot Placeholder

```text
images/docker-pull.png
```

<p align="center">

![Docker Pull](images/docker-pull.png)

</p>

---

# Step 17 — Deploy a Container Using the Private Image

Run a container from the image stored in Nexus.

```bash
docker run -d \
--name nginx-private \
-p 8080:80 \
<EC2-Public-IP>:<Docker-Repository-Port>/docker-hosted/nginx:1.0
```

Verify the running container.

```bash
docker ps
```

Open the application in a browser if applicable to confirm successful deployment.

---

## Screenshot Placeholder

```text
images/container-running.png
```

<p align="center">

![Container Running from Nexus Image](images/container-running.png)

</p>

---

# Complete Image Lifecycle

```text
Build Image
      │
      ▼
Tag Image
      │
      ▼
Docker Login
      │
      ▼
Push Image
      │
      ▼
Private Registry
      │
      ▼
Pull Image
      │
      ▼
Run Container
      │
      ▼
Application Running
```

This workflow demonstrates how Nexus Repository Manager acts as the central source of truth for container images.

---

# Deployment Verification Checklist

Confirm the following:

- Docker client authenticated successfully.
- Local image tagged correctly.
- Image pushed to Nexus.
- Image visible in the hosted repository.
- Image pulled successfully.
- Container started from the private image.
- Application verified after deployment.

---

# Screenshot Gallery

Replace each placeholder with your implementation screenshots.

| Activity | Screenshot |
|----------|------------|
| Docker Login | `images/docker-login.png` |
| Local Docker Images | `images/docker-images.png` |
| Docker Tag | `images/docker-tag.png` |
| Docker Push | `images/docker-push.png` |
| Images in Nexus | `images/repository-images.png` |
| Docker Pull | `images/docker-pull.png` |
| Running Container | `images/container-running.png` |

---

# Section Summary

At this stage, the complete private registry workflow has been successfully demonstrated:

- Docker client configured for authentication.
- Login to Nexus completed.
- Local Docker image tagged for the private registry.
- Image pushed to the Docker Hosted Repository.
- Image verified in the Nexus web interface.
- Image pulled from the private registry.
- Container deployed using the private image.

This validates the end-to-end process of securely storing, distributing, and deploying Docker images using Nexus Repository Manager hosted on AWS.

---

---

# Repository Management

After successfully configuring the private Docker registry, it is important to understand how Nexus Repository Manager supports long-term image management, version control, and operational maintenance.

Unlike a public registry, Nexus enables organizations to manage container images throughout the entire software development lifecycle.

---

# Managing Docker Images

A private registry serves as the central location for storing container images.

Typical image lifecycle:

```text
Developer

      │

Build Image

      │

Tag Image

      │

Push Image

      │

Store in Nexus

      │

Quality Assurance

      │

Production Deployment

      │

Archive Older Versions
```

This workflow promotes consistency across development, testing, and production environments.

---

# Image Versioning

Versioning is an essential practice when publishing Docker images.

Instead of relying solely on the `latest` tag, assign meaningful version numbers.

Examples:

```text
nginx:1.0

nginx:1.1

nginx:1.2

backend-api:2.0

frontend-app:3.4
```

Benefits include:

- Easier rollbacks
- Improved release management
- Better deployment consistency
- Clear version history

---

# Image Tagging Best Practices

Good tagging conventions improve maintainability.

Recommended formats include:

```text
application:1.0.0

application:2.1.5

application:production

application:staging

application:development
```

Avoid using `latest` as the only production tag.

Explicit version numbers make deployments more predictable.

---

# Repository Organization

As container images increase, organize repositories logically.

Example structure:

```text
docker-hosted

│

├── frontend

├── backend

├── nginx

├── redis

├── monitoring

└── utilities
```

Organized repositories simplify administration and improve discoverability.

---

# Security Best Practices

Private registries protect critical software assets.

Recommended practices include:

- Require authentication for all users.
- Use strong administrator credentials.
- Assign role-based permissions.
- Disable anonymous access unless explicitly required.
- Regularly review user accounts.
- Keep Nexus Repository Manager updated.
- Restrict registry access through security groups and firewalls.
- Back up repository data regularly.

These practices help strengthen the security of the software supply chain.

---

# Operational Best Practices

During this project, the following operational practices were applied:

- Store Nexus data in a Docker volume.
- Use descriptive repository names.
- Tag Docker images consistently.
- Validate every image after publishing.
- Document deployment procedures.
- Monitor repository storage usage.
- Remove obsolete images when appropriate.
- Verify successful image pulls before deployment.

---

# Monitoring Repository Health

Routine monitoring helps maintain a reliable registry.

Key areas to monitor include:

- Disk utilization
- Available storage
- Repository availability
- Docker image uploads
- Authentication failures
- Container status
- Docker daemon health

Regular monitoring supports stable operations and early issue detection.

---

# Backup Considerations

A private registry often contains critical deployment artifacts.

Recommended backup strategy:

```text
Nexus Repository

        │

Docker Volume

        │

Scheduled Backup

        │

Secure Storage

        │

Recovery
```

Backing up the Docker volume preserves repository configuration and stored images.

---

# Cleanup Procedures

Remove test containers.

```bash
docker rm -f nexus
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

Display Docker storage usage.

```bash
docker system df
```

Only remove the Nexus data volume if the repository is no longer required.

```bash
docker volume rm nexus-data
```

---

## Screenshot Placeholder

```text
images/cleanup.png
```

<p align="center">

![Cleanup](images/cleanup.png)

</p>

---

# Common Challenges

| Challenge | Resolution |
|-----------|------------|
| Docker login fails | Verify credentials and repository port |
| Image push denied | Confirm authentication and repository permissions |
| Cannot pull image | Verify image tag and repository endpoint |
| Nexus unavailable | Check container status and port configuration |
| Repository not visible | Verify repository creation and Docker connector |
| Container cannot start | Inspect logs and available system resources |

---

# Screenshot Walkthrough

Replace the placeholders below with screenshots from your implementation.

| Activity | Screenshot |
|----------|------------|
| EC2 Instance | `images/ec2-instance.png` |
| Docker Installed | `images/docker-installation.png` |
| Nexus Running | `images/nexus-running.png` |
| Nexus Login | `images/nexus-login.png` |
| Docker Hosted Repository | `images/docker-hosted-repository.png` |
| Docker Login | `images/docker-login.png` |
| Docker Image Tagging | `images/docker-tag.png` |
| Docker Push | `images/docker-push.png` |
| Repository Images | `images/repository-images.png` |
| Docker Pull | `images/docker-pull.png` |
| Running Container | `images/container-running.png` |
| Cleanup | `images/cleanup.png` |

---

# Project Summary

This project demonstrates the complete implementation of a private Docker registry using Nexus Repository Manager on AWS. The workflow covers deploying Nexus in Docker, configuring a Docker Hosted Repository, authenticating Docker clients, tagging images, publishing images to the private registry, retrieving those images, and deploying containers from trusted internal artifacts.

By centralizing container image storage, Nexus Repository Manager improves security, version management, and software distribution while reducing reliance on public registries. The project also reinforces best practices for registry administration, operational maintenance, documentation, and secure image management within modern DevOps workflows.

---

# Future Improvements

Potential enhancements include:

- Integrating Nexus with CI/CD pipelines.
- Automating image publishing from build systems.
- Enabling HTTPS with TLS certificates.
- Configuring role-based access control for development teams.
- Implementing image vulnerability scanning.
- Replicating repositories across environments.
- Integrating with Kubernetes deployments.
- Automating backup and disaster recovery.
- Monitoring registry performance with observability tools.

---

# Connect With Me

If you found this repository helpful or would like to discuss Docker, DevOps, Cloud Engineering, Infrastructure Automation, or Software Supply Chain Management, feel free to connect.

- **GitHub:** https://github.com/Chukwuemeka-Peter-Eze
- **LinkedIn:** https://www.linkedin.com/in/chukwuemekapetereze/
- **Notion:** https://lumpy-bubble-7b0.notion.site/Containers-with-Docker-3a546a96f97480a88041ff2ff82a6b5f?source=copy_link

If you found this project useful, consider giving it a ⭐ to support the repository.

---
