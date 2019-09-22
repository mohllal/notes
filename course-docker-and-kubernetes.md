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

### Section 2: Manipulating Containers with the Docker Client

- `docker run <image_name> <command>` command is used to override the default command of the Docker image.
- `docker ps`, and `docker container ls` commands are used to list all the running containers. `docker ps --all`, and `docker container ls --all` commands are used to list all containers that have ever created on the machine.
- `docker run <image_name>` = `docker create <image_name>` + `docker start <container_id>`.
- Creating a container is the process of preparing or setting up the file system snapshot to be used by this container.
- Running a container is the process of executing the startup command of the container.
- The `-a` flag used in `docker start` is used to make docker watch the output of the container and prints it out to the terminal.
- `docker system prune` command is used to remove:
  - All stopped containers
  - All networks not used by at least one container
  - All dangling images
  - All dangling build cache
- `docker container logs <container_id>` is used to get logs that have been emitted from a container.
- `docker container stop <container_id>` is used to stop a running container. It sends a `SIGTERM` (terminate signal) to the primary process running inside a container, giving it time to do some sort of clean-up before shutting down.
- `docker container kill <container_id>` is used to kill a running container. It sends a `SIGKILL` (kill signal) to the primary process running inside a container.
- After issuing `docker container stop` command; if a container does not automatically stop after 10 seconds, Docker will automatically fall back to issuing the `docker container kill` command on the container itself.
- `docker exec <container_id> <command>` is used to execute a certain command inside a container.
- Processes in linux environments have three communication channels attached to it:
  - STDIN: used to deliver information into the process
  - STDOUT: used to convey information out of the process
  - STDERR: used to convey errors out of the process
- `-it` flag is used to interactively provide input to a container. `-it` = `-i` + `-t`
- `-i` is used to attach the terminal to the STDIN channel of the running container.
- `-t` is used to format the entered/outputted text on the terminal.
- `sh` is a command processor or a shell program that is used to execute commands inside a container. Some examples of commands processors:
  - sh
  - zsh
  - powershell
  - bash
- `docker exec -it <container_id> ssh` is used to get full terminal access inside a running container.
