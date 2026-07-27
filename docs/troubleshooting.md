# Troubleshooting Guide

This document provides guidance for diagnosing and resolving common issues that may occur while deploying **Sonatype Nexus Repository Manager** as a private Docker registry in a local Docker environment.

Rather than listing only error messages, this guide explains how to identify the root cause of a problem, verify assumptions, and apply the appropriate resolution.

---

# Troubleshooting Workflow

When troubleshooting any deployment issue, follow a structured process.

```text
Problem Observed
       │
       ▼
Collect Information
       │
       ▼
Identify Root Cause
       │
       ▼
Apply Fix
       │
       ▼
Verify Resolution
       │
       ▼
Document Outcome
```

Using a consistent troubleshooting workflow reduces guesswork and improves reliability.

---

# Issue 1 — Nexus Web Interface Is Not Accessible

## Symptoms

* Browser cannot reach `http://localhost:8081`
* Connection refused
* Page continues loading indefinitely

## Possible Causes

* Nexus container is not running.
* Incorrect port mapping.
* Nexus is still starting.
* Docker daemon is unavailable.

## Verification

Check running containers.

```bash
docker ps
```

Check the Nexus logs.

```bash
docker logs nexus
```

Inspect the container configuration.

```bash
docker inspect nexus
```

## Resolution

* Ensure the container is running.
* Wait several minutes during the first startup.
* Verify that port **8081** is mapped correctly.
* Restart the container if necessary.

```bash
docker restart nexus
```

---

# Issue 2 — Unable to Retrieve the Administrator Password

## Symptoms

The following command fails.

```bash
docker exec nexus cat /nexus-data/admin.password
```

## Possible Causes

* Nexus has not finished initializing.
* Incorrect container name.
* Container is stopped.

## Verification

Confirm that the container is running.

```bash
docker ps
```

View startup logs.

```bash
docker logs nexus
```

## Resolution

Wait until Nexus completes its initialization and then run the command again.

---

# Issue 3 — Docker Login Fails

## Symptoms

```text
Error response from daemon
Unauthorized
Login failed
```

## Possible Causes

* Incorrect username or password.
* Wrong registry port.
* Docker Hosted Repository not configured.
* Docker connector disabled.

## Verification

Confirm the login command.

```bash
docker login localhost:8083
```

Verify that the repository is online in the Nexus web interface.

## Resolution

* Confirm your credentials.
* Verify the configured Docker connector port.
* Ensure the repository is online.
* Retry the login.

---

# Issue 4 — Image Push Is Denied

## Symptoms

```text
denied: requested access to the resource is denied
```

## Possible Causes

* Authentication failed.
* Incorrect image tag.
* Repository permissions.
* Wrong repository name.

## Verification

Display local images.

```bash
docker images
```

Confirm the tag.

Example:

```text
localhost:8083/docker-hosted/nginx:1.0
```

Verify authentication.

```bash
docker login localhost:8083
```

## Resolution

Retag the image if necessary.

```bash
docker tag nginx:latest \
localhost:8083/docker-hosted/nginx:1.0
```

Push again.

```bash
docker push localhost:8083/docker-hosted/nginx:1.0
```

---

# Issue 5 — Image Does Not Appear in Nexus

## Symptoms

The push command completes, but the image is not visible in the Nexus interface.

## Possible Causes

* Browser cache.
* Incorrect repository selected.
* Push failed before completion.

## Verification

Review the Docker push output.

Refresh the Nexus interface.

Verify that you are browsing the correct repository.

## Resolution

Repeat the push operation if necessary and confirm that all image layers upload successfully.

---

# Issue 6 — Unable to Pull an Image

## Symptoms

```text
manifest unknown

repository not found

pull access denied
```

## Possible Causes

* Incorrect image tag.
* Repository name mismatch.
* Image was never uploaded.
* Authentication expired.

## Verification

Confirm the image exists within the repository.

Verify the pull command.

```bash
docker pull localhost:8083/docker-hosted/nginx:1.0
```

## Resolution

* Confirm the repository name.
* Verify the image tag.
* Authenticate again.
* Retry the pull operation.

---

# Issue 7 — Container Fails to Start

## Symptoms

The container exits immediately after running.

## Verification

Inspect container logs.

```bash
docker logs <container-name>
```

Inspect container details.

```bash
docker inspect <container-name>
```

## Possible Causes

* Incorrect image.
* Port conflict.
* Invalid runtime configuration.

## Resolution

Resolve the underlying issue and recreate the container.

---

# Issue 8 — Port Already in Use

## Symptoms

Docker reports that a port is already allocated.

## Possible Causes

* Another container is using the port.
* Another application is listening on the same port.

## Verification

List running containers.

```bash
docker ps
```

## Resolution

Stop the conflicting container.

```bash
docker stop <container-name>
```

Or deploy Nexus using a different available port.

---

# Issue 9 — Nexus Startup Takes Longer Than Expected

## Explanation

During the first startup, Nexus initializes its internal database, configuration files, and repository structure.

This process may take several minutes.

## Verification

Monitor the logs.

```bash
docker logs -f nexus
```

## Resolution

Allow the initialization process to complete before attempting to access the web interface.

---

# Useful Diagnostic Commands

## Docker Information

```bash
docker info
```

---

## Running Containers

```bash
docker ps
```

---

## All Containers

```bash
docker ps -a
```

---

## Docker Images

```bash
docker images
```

---

## Docker Volumes

```bash
docker volume ls
```

---

## Docker Networks

```bash
docker network ls
```

---

## Docker Logs

```bash
docker logs nexus
```

---

## Docker Disk Usage

```bash
docker system df
```

---

## Docker System Cleanup

```bash
docker system prune
```

---

# Preventive Best Practices

Many deployment issues can be avoided by following a few operational practices:

* Verify Docker is running before starting the deployment.
* Use a persistent Docker volume for Nexus data.
* Wait for Nexus to finish initializing before logging in.
* Use consistent image naming and tagging.
* Verify authentication before pushing images.
* Confirm successful uploads in the Nexus interface.
* Monitor container logs during deployment.
* Clean up unused Docker resources regularly.

---

# Lessons from Troubleshooting

Troubleshooting is not simply about fixing errors—it is about understanding how the system behaves.

Throughout this project, several important principles became clear:

* Most deployment issues can be isolated through logs and inspection commands.
* Container status should always be verified before investigating application-level problems.
* Consistent naming and tagging reduce deployment mistakes.
* Incremental verification after each step makes failures easier to identify.
* Maintaining persistent storage prevents accidental data loss during container recreation.

Developing a systematic troubleshooting approach is an essential DevOps skill because it enables engineers to resolve issues efficiently while building confidence in the reliability of their infrastructure.
