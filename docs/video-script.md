# Video Script — Deploying Nexus Repository Manager as a Private Docker Registry

## Introduction

Hello everyone, and welcome.

In this project, I'll demonstrate how I deployed **Sonatype Nexus Repository Manager** as a **private Docker registry** using Docker in a local development environment.

The goal of this project was not simply to install Nexus, but to understand how container images are stored, managed, versioned, and distributed using a private artifact repository.

By the end of this walkthrough, you'll see how Docker clients authenticate with Nexus, publish container images, retrieve those images, and deploy containers using artifacts stored in a private registry.

Let's get started.

---

## Why This Project?

Most developers are familiar with Docker Hub, which is a public container registry.

However, in many organizations, applications contain proprietary code and cannot be published publicly.

Instead, companies deploy private registries where Docker images can be securely stored, versioned, and shared across development, testing, and production environments.

Nexus Repository Manager is one of the most widely used artifact repositories for this purpose.

It supports Docker images along with many other package formats such as Maven, npm, NuGet, PyPI, and Helm.

This project focuses specifically on using Nexus as a private Docker registry.

---

## Project Architecture

The architecture is intentionally simple.

Everything runs on a local machine using Docker.

The Docker Engine hosts a Nexus Repository Manager container.

Inside Nexus, a Docker Hosted Repository is created to store private Docker images.

The workflow looks like this:

Developer

↓

Docker Engine

↓

Nexus Repository Manager

↓

Docker Hosted Repository

↓

Docker Push / Docker Pull

↓

Container Deployment

Although this project uses a local environment, the same concepts apply when Nexus is deployed in production environments.

---

## Environment Preparation

The first step was verifying that Docker was installed and functioning correctly.

Once Docker was available, I created a persistent Docker volume called **nexus-data**.

This volume stores all repository configuration and uploaded artifacts independently of the container.

Separating persistent data from the container itself is an important DevOps practice because containers should remain replaceable while application data persists.

---

## Deploying Nexus

Next, I deployed Nexus Repository Manager as a Docker container.

The deployment mapped port **8081** to the local machine and attached the persistent Docker volume.

After starting the container, I verified that it was running correctly using Docker commands before accessing the web interface.

The first startup required several minutes while Nexus initialized its internal database and configuration.

---

## Initial Configuration

Once the web interface became available, I retrieved the temporary administrator password from inside the container.

After signing in, I created a new administrator password and completed the initial setup wizard.

At this point, the Nexus dashboard became available and the repository manager was ready for configuration.

---

## Creating the Docker Hosted Repository

The next step was creating a Docker Hosted Repository.

This repository serves as the private registry where Docker images are stored.

During configuration, I enabled the HTTP connector and selected the port that Docker clients would use when interacting with the registry.

Once the repository was created and online, Nexus was ready to receive Docker images.

---

## Authenticating Docker

Before Docker can publish images to a private registry, it must authenticate.

Using the Docker CLI, I logged into the Nexus registry with the administrator credentials.

A successful login confirmed that the Docker client could communicate with the repository securely.

Authentication is an important part of protecting software artifacts and ensuring that only authorized users can publish or retrieve images.

---

## Publishing Docker Images

With authentication complete, I selected a local Docker image and assigned it a new tag.

The new tag included:

* The registry endpoint
* Repository name
* Image name
* Version tag

After tagging the image, I pushed it to Nexus.

Docker uploaded each image layer individually, and once the upload completed, I verified that the image appeared inside the Docker Hosted Repository.

This confirmed that the private registry was functioning correctly.

---

## Retrieving and Deploying Images

To verify the complete workflow, I pulled the same image back from the repository.

Successfully retrieving the image demonstrated that Docker clients could consume artifacts stored in Nexus.

Finally, I deployed a container using the image retrieved from the private registry.

Opening the application in a browser confirmed that the deployment had completed successfully.

This completed the full lifecycle of publishing and consuming Docker images through a private registry.

---

## Challenges

One of the important lessons from this project was understanding that deploying Nexus is only one part of the process.

Container registries also require:

* Authentication
* Persistent storage
* Proper image tagging
* Repository organization
* Verification after every deployment step

Another lesson was the importance of using Docker logs and inspection commands when troubleshooting.

Rather than guessing, it's far more effective to verify container status, review logs, and identify the root cause before applying a fix.

---

## Key Takeaways

This project strengthened my understanding of:

* Docker container management
* Nexus Repository Manager
* Private Docker registries
* Artifact management
* Docker authentication
* Image versioning
* Persistent storage
* Technical documentation
* Operational troubleshooting

Perhaps the biggest takeaway was recognizing that a container registry is much more than a place to store images.

It becomes the trusted source of deployment artifacts used throughout the software delivery lifecycle.

---

## Future Improvements

There are several ways this project could be extended.

Future enhancements include:

* Integrating Nexus with CI/CD pipelines
* Enabling HTTPS with TLS certificates
* Configuring role-based access control
* Adding automated vulnerability scanning
* Deploying Nexus in Kubernetes
* Monitoring with Prometheus and Grafana
* Hosting the registry on cloud infrastructure

These improvements would bring the implementation closer to a production-ready deployment.

---

## Conclusion

Thank you for watching.

This project demonstrated how to deploy and configure Sonatype Nexus Repository Manager as a private Docker registry using Docker in a local development environment.

From deploying Nexus and configuring a Docker Hosted Repository to publishing images, retrieving them, and deploying containers, the project covered the complete workflow used to manage container artifacts.

I hope this walkthrough has been helpful.

If you'd like to explore the complete implementation, including documentation, architecture diagrams, and source files, feel free to visit the GitHub repository.

Thank you, and happy learning!
