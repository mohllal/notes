# Docker and Kubernetes: The Complete Guide

By: Stephen Grider. [Purchase the course](https://www.udemy.com/docker-and-kubernetes-the-complete-guide/)!

Date Started: Monday, September 21, 2019

Date Finished: Friday, ongoing

### Section 1: Dive into Docker

- Docker makes it really easy to install and run software without worrying about setup or dependencies.
- Docker is a platform/ecosystem about creating and managing containers.
- Docker ecosystem components:
  - Docker Server
  - Docker Client
  - Docker Images
  - Docker Compose
  - Docker Hub
  - Docker Machine
- Docker image is a single file containing all the dependencies and configurations required to run a specific program.
- Docker container is an instance of an image. It's a program with its own isolated set of hardware resources.
- Docker client/CLI is in charge of taking commands, processes them, and then communicating the commands over to the Docker server/demon.
- Running `docker run hello-world`, executes the following:
  1. The Docker client contacted the Docker daemon.
  2. The Docker server look at the image cache to check if there is a local copy of the `hello-world` image
  3. If not, the Docker daemon pulled the `hello-world` image from the Docker Hub and store it in the image cache.
  4. The Docker daemon created a new container, with name `hello-world`, from that image which runs the executable that produces the output message.
  5. The Docker daemon streamed that output to the Docker client, which sent it to the terminal.
- The OS kernel is a running software process, intermediate layer, that governs access between all programs running and all the physical hardware resources available in the compute.
- Namespacing is the process of isolating/segmenting hardware resources per process or group of processes.
- Control groups (cgroups) is the process of limiting the amount of resources user per process.
- Docker container is a process or group of process that have a specific group of resources assigned to it.
- Docker image is a filesystem snapshot along with some specific startup commands as well.
