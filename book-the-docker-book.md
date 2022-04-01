# The Docker Book

By: James Turnbull.
[Book Link](https://www.amazon.com/Docker-Book-Containerization-new-virtualization-ebook/dp/B00LRROTI4).

Date Started: Tuesday, September 24, 2019
Date Finished: Sunday, November 24, 2019

## Chapter 1: Introduction

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

## Chapter 2: Installing Docker

- Docker runs as a root-privileged daemon process to allow it to handle operations that can’t be executed by normal users (e.g., mounting filesystems).
- If a group named `docker` exists on our system, Docker will apply ownership of the Unix socket at `/var/run/docker.sock` to that group. Hence, any user that belongs to the `docker` group can run Docker without needing to use the `sudo` command.

## Chapter 3: Getting Started with Docker

- Docker has a ***client-server*** architecture. It has two binaries, the Docker server provided via the `dockerd` binary and the `docker` binary, that acts as a client. As a client, the `docker` binary passes requests to the ***Docker daemon*** (e.g., asking it to return information about itself), and then processes those requests when they are returned.
- Each Docker container has a network, IP address, and a bridge interface to talk to the local host.
- There are three ways containers can be identified: a ***short UUID*** (like `f7cbdac22a02`), a ***longer UUID*** (like `f7cbdac22a02e03c9438c729345e54db9d20cfa2ac1fc3494b6eb60872e74778`), and a ***name*** (like `gray_cat`).
- The `docker run` command with the `-d` flag tells Docker to detach the container to the background.
- To view the logs of a container without having to read the whole log file: `docker logs --tail 0 -f -t <docker_id>`.
- The `--log-driver` option controls the logging driver used by your daemon and container. There are a variety of options including the default `json-file` which provides the behavior we’ve just seen using the docker logs command.
- The `docker top` command is used to inspect the processes running inside the container.
- The `docker stats` command is used to shows statistics for one or more running Docker containers.
- The `--restart=always` flag checks for the container’s exit code and makes a decision whether or not to restart it.
- The `docker inspect` command will interrogate the container and return its configuration information, including names, commands, networking configuration, and a wide variety of other useful data.
- The ***/var/lib/docker*** directory holds the images, containers, and container configuration. All the containers in the ***/var/lib/docker/containers*** directory.
- The `docker rm -f ``sudo docker ps -a -q`` ` command will list all of the current containers using the `docker ps` command. The `-a` flag lists all containers, and the `-q` flag only returns the container IDs rather than the rest of the information about your containers. This list is then passed to the `docker rm` command, which deletes each container. The `-f` flag force removes any running containers.

## Chapter 4: Working with Docker images and repositories

- A Docker image is made up of ***filesystems*** layered over each other. At the base is a boot filesystem, `bootfs`, which resembles the typical Linux/Unix boot filesystem. A Docker user will probably never interact with the boot filesystem. When a container has booted, it is moved into memory, and the boot filesystem is unmounted to free up the ***RAM*** used by the `initrd` disk image.
- In Docker world, the root filesystem stays in ***read-only mode***, and Docker takes advantage of a ***union mount*** to add more read-only filesystems onto the root filesystem. The union mount overlays the filesystems on top of one another so that the resulting filesystem may contain files and subdirectories from any or all of the underlying filesystems.
- Images can be layered on top of one another. The image below is called the parent image and you can traverse each layer until you reach the bottom of the image stack where the final image is called the base image.
- When a container is launched from an image, Docker mounts a ***read-write*** filesystem on top of any layers below. This is where whatever processes we want our Docker container to run will execute.
- As changes occur, they are applied to the empty read-write layer; for example, if a change to file has happened, then that file will be copied from the read-only layer below into the readwrite layer. The read-only version of the file will still exist but is now hidden underneath the copy.
- Images live inside repositories, and repositories live on registries. The default registry is the public registry managed by Docker, Inc., [Docker Hub](https://hub.docker.com).
- Each image tag marks together a series of image layers that represent a specific image. This allows us to store more than one image inside a repository.
- There are two types of repositories: ***user repositories***, which contain images contributed by Docker users, and ***top-level repositories***, which are controlled by the people behind Docker.
- The `docker search` command is used search all of the publicly available images on Docker Hub.
- There are two ways to create a Docker image:
  - Via the `docker commit` command.
  - Via the `docker build` command with a `Dockerfile`.
- The `docker login` command is used to sign into the Docker Hub and and store the credentials for future use.
- The `docker logout` command is used to log out from a registry server.
- The `docker commit <container_id> <custom_image>` command is used to create a new custom image from a running container after changing its state. Of note is that the `docker commit` command only commits the differences between the image the container was created from and the current state of the container. This means updates are lightweight.
- The `-m` option which allows us to provide a commit message explaining our new image. And the `-a` option allows to list the author of the image.
- Docker will upload the ***build context***, as well as any files and directories contained in build context (the folder that contains the Dockerfile), to our Docker daemon when the build is run.
- The first instruction in a `Dockerfile` must be `FROM`. The `FROM` instruction specifies an existing image that the following instructions will operate on; this image is called the ***base image***.
- The `MAINTAINER` instruction, which tells Docker who the author of the image is and what their email address is. This is useful for specifying an owner and contact for an image.
- The `EXPOSE` instruction, which tells Docker that the application in this container will use this specific port on the container. That doesn’t mean you can automatically access whatever service is running on that port on the container. For security reasons, ***Docker doesn’t open the port automatically***.
- If a file named `.dockerignore` exists in the root of the build context then it is interpreted as a newline-separated list of exclusion patterns. Much like a `.gitignore` file it excludes the listed files from being treated as part of the build context, and therefore prevents them from being uploaded to the Docker daemon.
- Using the `--no-cache` flag with the `docker build` command will make Docker skip the cache.
- Specifying the `ENV` instruction to set an environment variable called `REFRESHED_AT`, showing when the template was last updated. When you want to refresh the build, you can change the date in the `ENV` instruction. Docker then resets the cache when it hits that `ENV` instruction and runs every subsequent instruction as a new without relying on the cache.
- The `docker history` command is used to drill down into how the image was created.
- The `CMD` instruction specifies the command to run when a container is launched. It is similar to the `RUN` instruction, but rather than running the command when the container is being built, it will specify the command to run when the container is launched.
- Only one `CMD` instruction in a `Dockerfile`. If more than one is specified, then the last `CMD` instruction will be used.
- Any arguments we specify on the `docker run` command line will be passed as arguments to the command specified in the `ENTRYPOINT`.
- The `WORKDIR` instruction provides a way to set the working directory for the container and the `ENTRYPOINT` and/or `CMD` to be executed when a container is launched from the image.
- The `ENV` instruction is used to set environment variables during the image build process.
- The `USER` instruction specifies a user that the image should be run as.
- The `VOLUME` instruction adds volumes to any container created from the image. A volume is a specially designated directory within one or more containers that bypasses the Union File System to provide several useful features for persistent or shared data.
- The `ADD` instruction adds files and directories from our build environment into our
image.
- The `COPY` instruction is closely related to the `ADD` instruction. The key difference is that the `COPY` instruction is purely focused on copying local files from the build context and does not have any extraction or decompression capabilities. You can't copy anything that is outside of this directory, because the build context is uploaded to the Docker daemon, and the copy takes place there. Anything outside of the build context is not available. The destination should be an absolute path inside the container.
- The `LABEL` instruction adds metadata to a Docker image. The metadata is in the form of ***key/value*** pairs.
- The `STOPSIGNAL` instruction instruction sets the system call signal that will be sent to the container when you tell it to stop. This signal can be a valid number from the kernel syscall table, for instance 9, or a signal name in the format `SIGNAME`, for instance `SIGKILL`.
- The `ARG` instruction defines variables that can be passed at build-time via the `docker build` command using the `--build-arg` flag.
- The `HEALTHCHECK` instruction tells Docker how to test a container to check that it is still working correctly. When a container has a health check specified, it has a ***health status*** in addition to its ***normal status***.
- The `docker inspect --format '{{.State.Health.Status}}'` command is used to see the state of the health check of a running container. The health check state and related data is stored in the `.State.Health` namespace and includes current state as well as a history of previous checks and their output.
- ***Automated Builds*** is the action of connecting a ***GitHub*** or ***BitBucket*** repository containing a `Dockerfile` to the Docker Hub. When a push to this repository happened, an image build will be triggered and a new image created. This was previously also known as a ***Trusted Build***.
- The `docker rmi <image_tag>` command deletes an image.
- Docker has ***open-sourced*** the code they use to run a ***Docker registry***, thus allowing us to build our own internal registry. The registry does not currently have a user interface and is only made available as an ***API service***.
- The `docker tag <source_image> <destination_image>` command is used to tag an existing image with another tag.

## Chapter 5: Testing with Docker

- Volumes are specially designated directories within one or more containers that ***bypass the layered Union File System*** to provide persistent or shared data for Docker. This means that changes to a volume are made directly and bypass the image. They will not be included when we commit or build an image.
- The `-v` option works by specifying a directory or mount on the local host separated from the directory on the container with a `:`. If the container directory doesn’t exist Docker will create it. Specifying the ***read/write*** status of the container directory is done by adding either `rw` or `ro` after container directory.
- There are two ways to make one container communicate with another one:
  - Through Docker’s own ***internal network***.
  - Using ***Docker Networking*** and the `docker network` command.
- The more realistic method for connecting containers is ***Docker Networking***:
  - Docker Networking can connect containers to each other ***across different hosts***.
  - Containers connected via Docker Networking can be stopped, started or restarted without needing to update connections.
  - There is no need to create a container before you can connect to it. You also don’t need to worry about the order in which you run containers and you get internal container name resolution and discovery inside the network.
- Every Docker container is ***assigned an IP address***, provided through an interface created when we installed Docker. That interface is called `docker0`. The `docker0` interface has an RFC1918 private IP address in the `172.16`-`172.30` range. This address `172.17.0.1`, will be the gateway address for the Docker network and all our Docker containers.
- The `docker0` interface is a virtual Ethernet bridge that connects our containers and the local host network. Every time Docker creates a container, it creates ***a pair of peer interfaces*** that are like opposite ends of a pipe (i.e., a packet sent on one will be received on the other). It gives ***one of the peers to the container*** to become its `eth0` interface and keeps the other peer, with a unique name like `vethec6` out on the host machine.
- By binding every `veth*` interface to the `docker0` bridge, Docker creates a ***virtual subnet*** shared ***between the host machine and every Docker container***.
- The `docker network` command creates a local, bridged network much like the `docker0` network. The `--net` flag specifies a network to run the container inside.
- A Docker network will also add the network name as a domain suffix for the network, any host in the network can be resolved by hostname.network_name.
- The `docker network disconnect` command is used to disconnect a container from a network.

## Chapter 6: Building services with Docker

- A volume is a specially designated directory within one or more containers that bypasses the Union File System to provide several useful features for persistent or shared data:
  - Volumes can be shared and reused between containers.
  - A container doesn’t have to be running to share its volumes.
  - Changes to a volume are made directly.
  - Changes to a volume will not be included when you update an image.
  - Volumes persist even when no containers use them.

## Chapter 7: Docker Orchestration and Service Discovery

- ***Docker Compose*** (previously Fig) is an open source Docker simple container *orchestration* tool.
- ***Consul*** is an open source *distributed service discovery* tool.
- ***Swarm*** is an open source Docker *orchestration and clustering* tool.
- With ***Docker Compose***, we define a set of containers to boot up, and their runtime properties, all defined in a `YAML` file. Docker Compose calls each of these containers ***services***.
- A distributed application is usually made up of multiple components. These components can be located together locally or distributed across data centers or geographic regions. Each of these components usually provides or consumes services to or from other components.
- ***Docker Swarm*** is native *clustering* for Docker. It turns a pool of ***Docker hosts*** into a single virtual Docker host. Swarm has a simple architecture. It clusters together multiple Docker hosts and serves the standard Docker API on top of that cluster.
- A *swarm* is a cluster of Docker hosts onto which you can deploy services. Since Docker 1.12 the Docker command line tool has included a swarm mode. This allows the *docker binary* to create and manage swarms as well as run local containers.
- A swarm is made up of ***manager*** and ***worker*** nodes. Manager do the dispatching and organizing of work on the swarm. Each unit of work is called a task. Managers also *handle all the cluster management functions* that keep the swarm *healthy* and *active*. You can have many manager nodes, if there is more than one then the manager node will conduct an *election for a leader*.
- ***Worker nodes*** run the tasks dispatched from ***manager nodes***. Out of the box, every node, managers and workers, will run tasks. You can instead configure a swarm manager node to *only perform management activities and not run tasks*.
- Swarm uses an abstraction, called a ***service*** as a building block. Services defined which tasks are executed on nodes. Each service consists of a *container image and a series of commands* to execute inside one or more containers on the nodes.
  - ***Replicated services*** - a swarm manager distributes replica tasks amongst workers according to a scale you specify.
  - ***Global services*** - a swarm manager dispatches one task for the service on every available worker.
- The `docker swarm init --advertise-addr` command is used to initialize a swarm and the `--advertise-addr` flag to specify the management IP of the new swarm.
- The `docker swarm join-token worker` command is used to get the registration token for the swarm.
- The `docker node ls` command is used to show the list of nodes in the swarm.
- The `docker swarm join` command takes a token, in our case the worker token,
 and the IP address and port of a Swarm manager node and adds that Docker host to the swarm.
- The `docker service create` command is used to create services that will be executed on our swarm nodes.
- The `--replicas` flag controls how many tasks are run on the swarm.
- The `docker service ls` command is used to list all services running on the swarm.
- The `docker service inspect` command is used to inspect a service on the swarm and show more info of it.
- The `docker service scale` command is used to add another task to the service, scaling it up.
- The `docker service rm` command is used to stop the service.

## Chapter 8: Using the Docker API

- There are three specific APIs in the Docker ecosystem.
  - The ***Registry API*** - provides integration with the Docker registry, which stores our images.
  - The ***Docker Hub API*** - provides integration with the Docker Hub.
  - The ***Docker Remote API*** - provides integration with the Docker daemon.
- By default, the Docker daemons binds to a socket, `unix:///var/run/docker.sock`, on the host on which it is running. The daemon runs with ***root privileges*** so as to have the access needed to manage the appropriate resources.
- The `-H` flag to specify the Docker host. Docker will also honor the `DOCKER_HOST` environment variable rather than requiring the continued use of the `-H` flag. Using `export DOCKER_HOST="tcp://docker.example.com:2375"`.
- `curl --unix-socket /var/run/docker.sock <endpoint> | python3 -mjson.tool`.
- The `http://docker/info` endpoint: Returns info about the Docker daemon running in the host. Much like the `docker info` command.
- The `http://docker/images/json` endpoint: Returns a list of all images on the Docker daemon. Much like the `docker images` command.
- The `http://docker/images/<image_id>/json` endpoint: Returns info about a specific image via its ID. Much like the `docker inspect` command.
- The `http://docker/images/search?term=<search_term>` endpoint: Returns a list of all images on the Docker Hub that contain a specific term. Much like the `docker search` command.
- The `http://docker/containers/json` endpoint: Returns a list of all running containers on the Docker daemon. Much like the `docker ps` command. To see running and stopped containers, we can add the `all` flag to the endpoint and set it to `1`.
- The `http://docker/containers/create` endpoint: Creates a container by using a POST request and a JSON hash containing an image name, hostname, etc.. And then a call to the `/containers/<container_id>/start` endpoint to start it. Much like the `docker run` command.
- The Remote API has an ***authentication*** mechanism that has been available since the 0.9 release of Docker. The authentication uses ***TLS/SSL certificates*** to secure your connection to the API.
- The `--tlsverify` flag is used to enable the TLS protection.
- The `--tlscacert=/etc/docker/ca.pem` `--tlscert=/etc/docker/server-cert.pem` `--tlskey=/etc/docker/server-key.pem` flags are used to specify CA certificate, certificate, and key.
- The `--tlsverify` flag enables the TLS connection to the Docker daemon.
