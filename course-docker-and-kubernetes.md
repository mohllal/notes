# Docker and Kubernetes: The Complete Guide

By: Stephen Grider. [Purchase the course](https://www.udemy.com/docker-and-kubernetes-the-complete-guide/)!

Date Started: Monday, September 21, 2019

Date Finished: Friday, ongoing

## Section 1: Dive into Docker

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

## Section 2: Manipulating Containers with the Docker Client

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

## Section 3: Client Building Custom Images Through Docker Server

- Dockerfile is a plain text file to put all configurations that define how the custom image should behave.
- Dockerfile line structure is as follows:
  - Instruction: tells the Docker server to do some specific preparations step on the custom image
  - Argument: customize how an instruction is executed.
- `FROM` instruction is used to specify the base docker image.
- `RUN` instruction is used to execute some command in the custom image
- `CMD` instruction is used to specify what should be executed when the custom image is used to start up a new container.
- The purpose of specifying a base image is giving us an initial set of programs that can be used to further customize our image.
- `.` in `docker build .` is used to specify the build context, the set of files and folders that belong to custom image.
- For every instruction included in the Dockerfile, and intermediate/temporary container is created.
- The `docker build` flow:
  - Take the image that was generated by the previous instruction and create a new container of it.
  - Execute a command in the newly created container or change its file system.
  - Take a snapshot of its file system and save it as an output for the next instruction along the chain.
  - The image that was generated during the last step is the output custom image.
- `docker build -t <docker_Id>/<repo_name>:<version>` command is used to tag an image.
- `docker commit -c <startup_command> <container_id>` command is used to manually generate a custom image from a running container.

## Section 4: Making Real Projects with Docker

- `alpine` is a term in the Docker world for an image that is as small and compact as possible.
- `COPY ./ ./` instruction is used to copy the contents of the build context folder to current working directory in the custom image.
- `-p <host_port>:<container_port>` flag is used to forward the traffic coming on a port inside the host network to a port inside the container network.
- `WORKDIR <folder_path>` instruction is used to change the working directory in the container.

## Section 5: Docker Compose with Multiple Local Containers

- `docker-compose` is a separate CLI tool that gets installed along with Docker. It is used to start up multiple containers at the same time and automate some of the long-winded flags passed to the `docker run` command.
- `docker-compose up` = `docker run <image_id>`
- `docker-compose up --build` = `docker build .` + `docker container run <image_id>`
- `docker-compose up --build` command is used to shut down all the running containers generated by the `docker-compose.yaml` file.
- There are four restart policies available in docker-compose:
  - *no*: The default restart policy. Never attempt to restart that container if it stops or crashes.
  - *always*: Restart the container if it stops for any reason. Always attempt to restart it.
  - *on-failure*: Only restart the container if it stops with an error code.
  - *unless-stopped*: Always restart the container unless someone forcibly stop it.
- `docker-compose ps` command is used to list all containers that are running by issuing a `docker-compose up` command.

## Section 6: Creating a Production-Grade Workflow

- `docker build -f <file_name> .` command is used to build a docker image from a `Dockerfile` with a custom name.
- `-v <host_path>:<container_path>` flag is used to mount a volume of a folder/file in the host machine to a folder/file in the container.
- `dockerfile` instruction in the `docker-compose.yaml` file is used to override the default path for the default `Dockerfile` used to build the custom image.
- `docker attach <container_id>` command is used to attach the terminal to the STDIN, STDOUT and STDERR of the primary process, process with PID equal to `1`, running in the container.

## Section 12: Onwards to Kubernetes

- ***Kubernetes*** is system for running many different Docker ***Containers***, so different types of Containers, or different number of Containers over multiple different machines.
- A ***Cluster*** in the world of *Kubernetes* is the assembly of something called a ***Master*** and one or more ***Nodes***; A *Node* is a ***virtual*** or ***physical*** machine that is going to be used to run some number of different Docker ***Containers***.
- Kubernetes *Master* has a set of different programs running on it that control what each of *Nodes* are running at any given time.
- `minikube` is a command line tool whose solo purpose is going to be set up a tinny littler *Kubernetes* cluster on local environment.
- ***Managed Solutions*** is a reference to outside cloud providers such as *Google Cloud* or *Amazon Web Services (AWS)* that will set up an entire *Kubernetes* cluster and take care of a lot of very low-level tasks that are required to get everything work in a secure fashion.
  - ***Amazon Elastic Container Service for Kubernetes (EKS)***
  - ***Google Cloud Kubernetes Engine (GKE)***
- `kubectl` is a program that is used to interact with a ***Kubernetes Cluster*** in general and manage what all the different *Nodes* are doing and what different *Containers* they are running.
- Configuration files are used to create ***Kubernetes Objects*** which serve different purposes - running a container, monitoring a container, setting up networking, etc..:
  - ***StatefulSet***
  - ***ReplicaController***
  - ***Pod***
  - ***Service***
- Each API version defines a different set of *Kubernetes Objects* we can use.
- A `Pod` is essentially a group of containers with a very command purpose, it's the smallest *Kubernetes Object* that can be deployed to run one or more containers inside on it. In a `Pod` we group together containers that have a *very discrete*, *very tightly coupled* relationship.
- A `Service` *Kubernetes Object* is used to setup networking in a *Kubernetes Cluster*. There are four commonly used `Service` subtypes:
  - ***ClusterIP***
  - ***NodePort***
  - ***LoadBalancer***
  - ***Ingress***
- The purpose of `NodePort` service is to expose a *Container* to the outside world.
- Every single *Kubernetes Node* has a program on it called `kube-proxy` which is the one and single window to the outside world. Any time a request comes into the *Node*, it's going to pass through the `kube-proxy` proxy which going to inspect the request and decide how to route it to different services or different `Pod`s.
- The `kubectl apply -f <file_name>` command is used to feed a configuration file to `Kubectl`.
- The `kubectl get pods` command is used to print the status of all running *Pods*.
- There are two ways of approaching deployments:
  - ***Imperative Deployment*** (*discrete commands or actions*)
  - **Declarative Deployment** (*guidelines*)