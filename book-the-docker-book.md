# The Docker Book

By: James Turnbull. [Purchase the book](https://dockerbook.com/)!

Date Started: Tuesday, September 24, 2019

Date Finished: ongoing

### Chapter 1: Introduction

- Container virtualization is often called operating system-level virtualization. Container technology allows multiple isolated user space instances to be run on a single host.
- In Docker, having modern Linux kernel features, such as control groups and namespaces, means that containers can have strong isolation, their own network and storage stacks, as well as resource management capabilities to allow friendly co-existence of multiple containers on a host.
- Docker is an open-source engine that automates the deployment of applications
into containers. Docker’s mission is to provide:
  - An easy and lightweight way to model reality
  - A logical segregation of duties
  - Fast, and efficient development life cycle
  - Encourages service oriented and microservices architecture
- Docker components:
  - The Docker client and server, also called the Docker Engine.
  - Docker Images
  - Images Registries
  - Docker Containers
- Docker is a client-server application. The Docker client talks to the Docker server
or daemon, which, in turn, does all the work. Docker ships with a command line client binary, `docker`, as well as a full *RESTful API* to interact with the daemon: `dockerd`.
- Images are the *build* part of Docker’s life cycle. They are a ***layered format***, using Union file systems, that are built step-by-step using a series of instructions.
- Docker stores the images in registries. There are two types of registries: public and private. The public registry for images, called the [Docker Hub](https://hub.docker.com).
- Each container contains a software image -- its *cargo* -- and, like its physical counterpart, allows a set of operations to be performed. For example, it can be created, started, stopped, restarted, and destroyed.
- The Docker ecosystem contains two more tools:
  - Docker Compose: which allows to run stacks of containers to represent application stacks.
  - Docker Swarm: which allows to create clusters of containers, called swarms, that allow you to run scalable workloads.

### Chapter 2: Installing Docker

- Docker runs as a root-privileged daemon process to allow it to handle operations that can’t be executed by normal users (e.g., mounting filesystems).
- If a group named `docker` exists on our system, Docker will apply ownership of the Unix socket at `/var/run/docker.sock` to that group. Hence, any user that belongs to the `docker` group can run Docker without needing to use the `sudo` command.
