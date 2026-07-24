# Project Demonstration Video Script

**Project:** Nexus Repository Manager as a Private Docker Registry on AWS

**Target Duration:** 10–12 Minutes

**Target Audience**

- Recruiters
- Hiring Managers
- DevOps Engineers
- Cloud Engineers
- Platform Engineers
- Site Reliability Engineers (SREs)
- Infrastructure Engineers

---

# Video Objective

The objective of this demonstration is to showcase the deployment and configuration of **Nexus Repository Manager** as a **private Docker registry** running on an AWS EC2 instance.

The demonstration covers:

- Deploying Nexus Repository Manager
- Configuring a Docker Hosted Repository
- Authenticating Docker clients
- Publishing Docker images
- Retrieving Docker images
- Running containers from the private registry
- Explaining private image management workflows
- Demonstrating enterprise DevOps best practices

---

# Scene 1 — Introduction (45 Seconds)

## Screen

Open the GitHub repository homepage.

Highlight:

- README
- Project structure
- Architecture diagram

### Narration

> Hello everyone, and welcome.

> In this project, I demonstrate how to deploy and configure Nexus Repository Manager as a private Docker registry on AWS.

> Instead of relying exclusively on public registries, organizations commonly store internal container images inside private repositories where access can be controlled and images can be managed securely.

> Throughout this project, I documented the complete implementation, architecture, deployment process, troubleshooting steps, and operational best practices.

---

# Scene 2 — Repository Walkthrough (1 Minute)

## Screen

Scroll through the repository.

Highlight:

- README
- Setup Guide
- Commands Guide
- Troubleshooting Guide
- Lessons Learned
- Architecture Diagram

### Narration

> The repository is organized to make the implementation easy to reproduce.

> Each document explains a different aspect of the deployment, from preparing the AWS environment to managing Docker images inside the private registry.

---

# Scene 3 — Architecture Diagram (1 Minute)

## Screen

Open

```text
images/architecture.png
```

Zoom into the diagram.

### Narration

> This architecture illustrates how Docker clients interact with Nexus Repository Manager hosted on an AWS EC2 instance.

> Docker images are pushed into the private registry, stored securely, and later pulled by authorized users or deployment systems.

> Nexus serves as the central repository for managing container images throughout the software delivery lifecycle.

---

# Scene 4 — AWS Environment (1 Minute)

## Screen

Show:

- AWS EC2 Console
- Ubuntu instance
- Docker installed

### Narration

> The implementation was completed on an Ubuntu EC2 instance running in AWS.

> Docker Engine provides the container runtime, while Nexus Repository Manager runs as a Docker container with persistent storage using a Docker volume.

---

# Scene 5 — Deploy Nexus Repository Manager (1 Minute)

## Screen

Show terminal.

Run or display:

```bash
docker volume create nexus-data
```

Then:

```bash
docker run -d \
--name nexus \
-p 8081:8081 \
-v nexus-data:/nexus-data \
sonatype/nexus3
```

Display:

```bash
docker ps
```

### Narration

> First, a persistent Docker volume is created to store repository data.

> Nexus Repository Manager is then deployed as a Docker container using that persistent volume.

> This approach separates repository data from the container lifecycle, allowing upgrades or container recreation without losing stored images.

---

# Scene 6 — Configure Nexus (1 Minute)

## Screen

Open the Nexus web interface.

Show:

- Login page
- Dashboard
- Docker Hosted Repository

### Narration

> After deployment, the administrator password is retrieved from inside the container.

> Once logged in, a Docker Hosted Repository is created to receive Docker images published from authenticated clients.

---

# Scene 7 — Docker Authentication (1 Minute)

## Screen

Show terminal.

Run:

```bash
docker login
```

Display successful authentication.

### Narration

> Before Docker can publish images, it must authenticate with the private registry.

> Successful authentication confirms that the client is authorized to push and pull images.

---

# Scene 8 — Publish a Docker Image (2 Minutes)

## Screen

Display:

```bash
docker images
```

Tag the image.

```bash
docker tag
```

Push the image.

```bash
docker push
```

Open Nexus.

Show the uploaded image.

### Narration

> The Docker image is first tagged using the Nexus registry endpoint.

> After tagging, the image is pushed to the hosted repository.

> Once the upload completes, the image becomes visible inside Nexus, confirming successful publication.

---

# Scene 9 — Pull the Image (1 Minute)

## Screen

Display:

```bash
docker pull
```

Run a new container.

```bash
docker run
```

Display:

```bash
docker ps
```

### Narration

> The image is then downloaded from the private registry.

> Finally, a new container is started using the image stored inside Nexus, demonstrating the complete publish-and-deploy workflow.

---

# Scene 10 — Lessons Learned (1 Minute)

## Screen

Open the "Lessons Learned" section.

### Narration

> This project reinforced several important DevOps concepts, including private container registries, Docker authentication, image versioning, persistent storage, secure software distribution, and the importance of infrastructure documentation.

> It also demonstrated how centralized image management improves consistency across development and deployment environments.

---

# Scene 11 — Conclusion (45 Seconds)

## Screen

Return to the repository homepage.

### Narration

> Thank you for watching this project demonstration.

> This repository documents the complete process of deploying Nexus Repository Manager as a private Docker registry on AWS, from infrastructure preparation to secure image management.

> Feedback and suggestions are always welcome. Thank you for your time.

---

# Recording Checklist

Before recording, verify the following:

- Docker Engine is running.
- Nexus container is healthy.
- Docker Hosted Repository is configured.
- Docker login succeeds.
- Image tagging completed.
- Image successfully pushed.
- Image visible in Nexus.
- Image successfully pulled.
- Container starts from the private image.
- Architecture diagram included.
- Screenshots added to the repository.
- Terminal output is readable.
- Sensitive information (passwords, tokens, IPs if desired) is hidden or redacted.
- Desktop notifications are disabled.

---

# Suggested Repository Assets

Include the following assets:

- `architecture.png`
- EC2 instance screenshot
- Docker installation screenshot
- Nexus container running
- Nexus login page
- Nexus dashboard
- Docker Hosted Repository configuration
- Docker login success
- Docker image tagging
- Docker image push
- Repository image listing
- Docker image pull
- Running container
- Cleanup commands
- Animated GIF showing the complete push → pull workflow
- Repository banner image

---

# Estimated Timeline

| Section | Duration |
|----------|----------|
| Introduction | 0:45 |
| Repository Overview | 1:00 |
| Architecture | 1:00 |
| AWS Environment | 1:00 |
| Nexus Deployment | 1:00 |
| Nexus Configuration | 1:00 |
| Docker Authentication | 1:00 |
| Push Image | 2:00 |
| Pull Image & Run Container | 1:00 |
| Lessons Learned | 1:00 |
| Conclusion | 0:45 |

**Estimated Total:** **10–12 minutes**

---

# Final Notes

This demonstration should emphasize not only the commands used, but also the reasoning behind each step. Explain how a private registry improves security, centralizes image management, and supports reliable software delivery. Combining the architecture diagram, AWS deployment, live demonstrations, troubleshooting insights, and documentation will present a complete, professional portfolio project that reflects practical experience with private container registries and modern DevOps workflows.
