# Troubleshooting Guide

This document outlines common issues that may occur while deploying and using Nexus Repository Manager as a private Docker registry on AWS. It also provides practical troubleshooting steps and recommended resolutions.

---

# Table of Contents

- Docker Installation Issues
- Nexus Container Issues
- Nexus Web Interface Issues
- Authentication Problems
- Docker Push Issues
- Docker Pull Issues
- Repository Configuration Issues
- Docker Networking Issues
- Performance Issues
- Persistent Storage Issues
- AWS Networking Issues
- Diagnostic Commands

---

# Docker Installation Issues

## Problem

Docker commands fail with messages such as:

```text
docker: command not found
```

## Possible Causes

- Docker is not installed.
- Docker installation failed.
- PATH is not configured correctly.

## Resolution

Verify Docker installation.

```bash
docker --version
```

Check Docker service status.

```bash
sudo systemctl status docker
```

If Docker is not running:

```bash
sudo systemctl start docker
```

Enable Docker at boot.

```bash
sudo systemctl enable docker
```

---

# Nexus Container Does Not Start

## Symptoms

```bash
docker ps
```

returns no running Nexus container.

## Diagnosis

Check all containers.

```bash
docker ps -a
```

Inspect logs.

```bash
docker logs nexus
```

## Possible Causes

- Insufficient memory.
- Port conflict.
- Container startup failure.
- Invalid Docker command.

## Resolution

Restart the container.

```bash
docker restart nexus
```

If necessary:

```bash
docker rm nexus
```

Deploy a new container.

---

# Nexus Web Interface Is Unavailable

## Symptoms

The browser cannot reach:

```text
http://<EC2-Public-IP>:8081
```

## Possible Causes

- Nexus is still starting.
- Security group blocks port 8081.
- Container stopped.
- Incorrect public IP address.

## Resolution

Verify the container.

```bash
docker ps
```

Review logs.

```bash
docker logs nexus
```

Confirm that port **8081** is allowed in the AWS security group.

---

# Unable to Retrieve Administrator Password

## Symptoms

```bash
docker exec nexus cat /nexus-data/admin.password
```

returns an error.

## Possible Causes

- Nexus has not completed initialization.
- Incorrect container name.
- Container stopped.

## Resolution

Confirm the container is running.

```bash
docker ps
```

Wait several minutes after deployment and retry the command.

---

# Docker Login Fails

## Symptoms

```text
unauthorized

authentication failed
```

## Possible Causes

- Incorrect username or password.
- Incorrect Docker registry port.
- Docker Hosted Repository not configured.
- Docker connector not enabled.

## Resolution

Verify credentials.

Retry login.

```bash
docker login <EC2-Public-IP>:<Registry-Port>
```

Confirm the repository endpoint matches the configured Docker connector.

---

# Docker Push Fails

## Symptoms

```text
denied

requested access denied

unauthorized
```

## Possible Causes

- Authentication failed.
- Incorrect image tag.
- Repository permissions.
- Wrong registry endpoint.

## Resolution

Verify login.

```bash
docker login
```

Display image tags.

```bash
docker images
```

Retag the image if necessary.

```bash
docker tag
```

Retry the push.

```bash
docker push
```

---

# Docker Pull Fails

## Symptoms

```text
manifest unknown

image not found

repository does not exist
```

## Possible Causes

- Incorrect image tag.
- Incorrect repository.
- Image was never pushed.
- Authentication required.

## Resolution

Verify the image exists in Nexus.

Confirm the image tag.

Retry:

```bash
docker pull
```

---

# Repository Does Not Appear

## Symptoms

The Docker Hosted Repository is missing from Nexus.

## Possible Causes

- Repository was not created.
- Repository configuration failed.
- Browser caching.

## Resolution

Navigate to:

```text
Repositories
```

Verify the repository exists.

If necessary, recreate the repository.

---

# Docker Networking Problems

## Symptoms

Docker cannot communicate with Nexus.

## Diagnosis

Display Docker networks.

```bash
docker network ls
```

Inspect the default network.

```bash
docker network inspect bridge
```

Verify the registry endpoint.

---

# Port Conflict

## Symptoms

Container startup fails.

## Diagnosis

Display port usage.

```bash
sudo ss -tulpn
```

or

```bash
sudo netstat -tulpn
```

## Resolution

Stop the conflicting service or configure Nexus to use a different port.

---

# Docker Volume Issues

## Symptoms

Repository data disappears after recreating the container.

## Possible Causes

- Volume not mounted.
- Incorrect mount path.
- Volume deleted.

## Diagnosis

Inspect the volume.

```bash
docker volume inspect nexus-data
```

Inspect the container.

```bash
docker inspect nexus
```

Verify the mounted volume.

---

# AWS Connectivity Issues

## Symptoms

SSH works, but Nexus cannot be reached.

## Possible Causes

- Security Group configuration.
- Incorrect public IP.
- Network ACL restrictions.
- Firewall configuration.

## Resolution

Verify inbound rules for:

- SSH (22)
- Nexus Web Interface (8081)
- Docker Repository Port

Confirm the EC2 instance is running and accessible.

---

# Performance Issues

## Symptoms

Nexus starts slowly or becomes unresponsive.

## Possible Causes

- Limited memory.
- High CPU utilization.
- Large repository.
- Insufficient disk space.

## Resolution

Monitor system resources.

```bash
free -h
```

```bash
top
```

```bash
df -h
```

Increase EC2 resources if necessary.

---

# Useful Diagnostic Commands

Display running containers.

```bash
docker ps
```

Display all containers.

```bash
docker ps -a
```

View Nexus logs.

```bash
docker logs nexus
```

Inspect the Nexus container.

```bash
docker inspect nexus
```

List Docker images.

```bash
docker images
```

List Docker volumes.

```bash
docker volume ls
```

Inspect a Docker volume.

```bash
docker volume inspect nexus-data
```

Display Docker networks.

```bash
docker network ls
```

Display Docker storage usage.

```bash
docker system df
```

Monitor Docker events.

```bash
docker events
```

---

# Troubleshooting Workflow

```text
Issue Detected
      │
      ▼
Identify Symptoms
      │
      ▼
Review Docker Logs
      │
      ▼
Inspect Container
      │
      ▼
Verify Repository Configuration
      │
      ▼
Check AWS Networking
      │
      ▼
Validate Docker Authentication
      │
      ▼
Retry Operation
      │
      ▼
Confirm Successful Resolution
```

---

# Conclusion

Most deployment issues can be resolved by systematically checking Docker services, Nexus container status, repository configuration, authentication settings, networking, and persistent storage. Using Docker's inspection and logging commands together with AWS network verification provides a structured approach to diagnosing and resolving common problems encountered when operating a private Docker registry.
