# Docker-Fundamentals
## 📌 Overview

Docker is a containerization platform used to package and run applications in isolated environments called **containers**.

A Docker container contains the application, dependencies, libraries, and required configuration needed to run the application.

### Basic Docker Flow

```text
Dockerfile
    ↓
Docker Build
    ↓
Docker Image
    ↓
Docker Container
```

---

# 1. 🛠️ Docker Installation on Amazon Linux 2023

### Launch an EC2 Instance

Launch an **Amazon Linux 2023** EC2 instance.

### Install Docker

```bash
yum install docker -y
```

### Start Docker

```bash
systemctl start docker
```

### Check Docker Service

```bash
systemctl status docker
```

### Check Docker Version

```bash
docker -v
```

---

# 2. 📦 Important Docker Commands

### List Docker Images

```bash
docker images
```

Shows all Docker images available on the local machine.

### List Running Containers

```bash
docker ps
```

### List All Containers

```bash
docker ps -a
```

Shows both **running and stopped containers**.

---

# 3. 🖼️ Docker Images

A **Docker Image** is a read-only template or blueprint used to create containers.

An image can contain:

* Application code
* Runtime
* Libraries
* Dependencies
* Configuration
* Required OS components

### Two Ways to Get Docker Images

#### Method 1 – Pull from Docker Hub

```bash
docker pull ubuntu
```

```bash
docker pull amazonlinux
```

#### Method 2 – Create an Image Using Dockerfile

```text
Dockerfile
    ↓
docker build
    ↓
Docker Image
```

---

# 4. 🧱 Docker Image Layers

A Docker image is made up of multiple **layers**.

Each layer represents a set of filesystem changes, such as:

* Adding files
* Modifying files
* Installing packages
* Deleting files

Layers help Docker with:

* Storage optimization
* Reusability
* Faster image builds
* Image caching

---

# 5. 📦 Creating a Container

Pull an Amazon Linux image:

```bash
docker pull amazonlinux
```

Create and enter a container:

```bash
docker run -it --name amazonlinuxcontainer amazonlinux
```

### Meaning

```text
docker run
    ↓
Create + Start Container

-it
    ↓
Interactive Terminal

--name amazonlinuxcontainer
    ↓
Container Name

amazonlinux
    ↓
Image Name
```

Inside the container:

```bash
ls
```

Install software:

```bash
yum install git maven tree httpd -y
```

Check versions:

```bash
git --version
mvn --version
tree --version
```

Create files:

```bash
touch file{1..10}
```

---

# 6. 🚪 Exit From a Container

### Exit Without Stopping the Container

Press:

```text
CTRL + P
CTRL + Q
```

The container continues running in the background.

### Exit and Stop the Container

```bash
exit
```

or:

```text
CTRL + D
```

---

# 7. 🔄 Run Container in Detached Mode

If you don't want to enter the container immediately:

```bash
docker run -it -d --name ubuntucontainer ubuntu
```

### `-d`

Means:

```text
Detached Mode
```

The container runs in the background.

Check:

```bash
docker ps
```

---

# 8. 🔗 Docker Attach

`docker attach` connects your terminal to the **main process** of a running container.

Example:

```bash
docker attach ubuntucontainer
```

or:

```bash
docker attach <container-id>
```

Inside the container:

```bash
apt update
apt install nginx -y
```

### Important

`docker attach` connects to the container's **main process (PID 1)**.

If you use:

```bash
exit
```

the main process may terminate and the container can stop.

---

# 9. ⚡ Docker Exec

`docker exec` runs a **new command/process** inside an already running container.

Example:

```bash
docker exec -it 1275f407dc04 /bin/bash
```

Inside the container:

```bash
apt update
apt install git -y
apt install apache2 -y
```

Start Apache:

```bash
service apache2 start
```

Check Apache:

```bash
service apache2 status
```

Exit:

```text
CTRL + P
CTRL + Q
```

### Why `exec` is Preferred

```text
docker attach
      ↓
Connects to MAIN process

docker exec
      ↓
Starts a NEW process
```

Therefore, `docker exec` is generally preferred for accessing a running container.

---

# 10. 🔍 Attach vs Exec

| Feature                   | Docker Attach                  | Docker Exec                      |
| ------------------------- | ------------------------------ | -------------------------------- |
| Purpose                   | Connect to main process        | Run a new process                |
| Process                   | Main process / PID 1           | Separate process                 |
| Container must be running | Yes                            | Yes                              |
| Can open shell            | Yes                            | Yes                              |
| `exit` can stop container | Yes, depending on main process | No                               |
| Common usage              | Interact with main process     | Troubleshooting / administration |

### Example

```bash
docker attach mycontainer
```

```bash
docker exec -it mycontainer /bin/bash
```

---

# 11. 🐳 Useful Docker Container Commands

### Search Images

```bash
docker search ubuntu
```

### List All Containers

```bash
docker ps -a
```

### List Running Containers

```bash
docker ps
```

### Stop a Container

```bash
docker stop testcontainer
```

The container enters the **Exited** state.

### Start a Container

```bash
docker start testcontainer
```

### Restart a Container

```bash
docker restart testcontainer
```

### Pause a Container

```bash
docker pause testcontainer
```

Pauses processes inside the container.

### Resume a Container

```bash
docker unpause testcontainer
```

### Inspect Container

```bash
docker inspect testcontainer
```

Shows detailed information about the container.

### Kill a Container

```bash
docker kill testcontainer
```

Forcefully stops the container.

### Remove a Container

```bash
docker rm testcontainer
```

### Force Remove a Container

```bash
docker rm -f testcontainer
```

### View Container Logs

```bash
docker logs testcontainer
```

### Show Processes

```bash
docker top testcontainer
```

### View Resource Usage

```bash
docker stats testcontainer
```

### Show Filesystem Changes

```bash
docker diff testcontainer
```

---

# 12. 📂 Copy Files Between Host and Container

Use `docker cp` to copy files.

### Host → Container

```bash
docker cp hello.txt cont1:/tmp
```

Example:

```text
Local Machine
     │
     │ docker cp
     ↓
Container
     │
     └── /tmp/hello.txt
```

---

# 13. 💾 Save Docker Image to TAR File

Save an image:

```bash
docker save ubuntu > ubuntu.tar
```

This creates:

```text
ubuntu.tar
```

### Load the Image

```bash
docker load -i ubuntu.tar
```

This is useful for transferring Docker images between systems without pulling them again from Docker Hub.

---

# 14. 📜 Docker Image History

To see how an image was built:

```bash
docker history ubuntu
```

---

# 15. 🧹 Remove Unused Images

```bash
docker image prune
```

Removes unused Docker images according to Docker's pruning rules.

---

# 16. ⏸️ STOP vs KILL

### Docker Stop

```bash
docker stop container
```

Docker asks the process to terminate gracefully and waits for it to finish.

### Docker Kill

```bash
docker kill container
```

Docker immediately terminates the container process.

### Simple Difference

```text
STOP
 ↓
Graceful termination
 ↓
Waits for process

KILL
 ↓
Immediate termination
```

---

# 17. 🏗️ Create a Container Without Starting It

```bash
docker create -it ubuntu
```

This creates the container but does not start it.

---

# 18. ✏️ Rename a Container

```bash
docker rename old-name new-name
```

Example:

```bash
docker rename ubuntucontainer myubuntu
```

---

# 19. 🖼️ Docker Image – Detailed Explanation

A Docker image is a **read-only template** from which containers are created.

Think of it like a **blueprint**.

```text
              Docker Image
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
    Container   Container   Container
```

One image can be used to create multiple containers.

When a container is created, Docker adds a **writable container layer** on top of the read-only image layers.

```text
Container
──────────────
Writable Layer
──────────────
Image Layer
──────────────
Image Layer
──────────────
Base Layer
```

---

# 20. 🧪 Docker `commit`

A container can be converted into a new Docker image using:

```bash
docker commit <container> <image-name>
```

---

# 21. 🛠️ Practical Task – Create a Custom Image

### Step 1 – Pull Ubuntu

```bash
docker pull ubuntu
```

### Step 2 – Create Container

```bash
docker run -it --name ubuntucontainer ubuntu
```

### Step 3 – Update Packages

Inside the container:

```bash
apt update
```

### Step 4 – Install Software

```bash
apt install apache2 python3 mysql-server -y
```

### Step 5 – Leave the Container Running

```text
CTRL + P
CTRL + Q
```

### Step 6 – Create a New Image

From the Docker host:

```bash
docker commit ubuntucontainer myimagewithapachenmysql
```

### Step 7 – Verify the Image

```bash
docker images
```

You should see:

```text
myimagewithapachenmysql
```

---

# 22. 🚀 Create a Container From Custom Image

```bash
docker run -it -d \
--name mycontwithapachemysql \
myimagewithapachenmysql
```

Access the container:

```bash
docker exec -it mycontwithapachemysql /bin/bash
```

Check MySQL:

```bash
mysql --version
```

---

# 23. 🔄 What We Did With `docker commit`

The process was:

```text
Ubuntu Image
     ↓
Create Container
     ↓
Install Apache
Python
MySQL
     ↓
Container with Software
     ↓
docker commit
     ↓
Custom Docker Image
     ↓
Create New Containers
```

### Important

This approach is **manual**.

For repeatable and automated image creation, we should use a **Dockerfile**.

---

# 24. ❌ Why Manual Image Creation Is Not Recommended

Using:

```bash
docker run
docker exec
apt install ...
docker commit
```

is difficult to maintain because:

* Manual steps are required
* Hard to reproduce consistently
* Easy to make mistakes
* Difficult to version-control
* Difficult to automate

### Better Approach

Use:

```text
Dockerfile
     ↓
docker build
     ↓
Docker Image
     ↓
Container
```

---

# 25. 🧹 Docker Cleanup

### List Containers

```bash
docker ps -a
```

### Get Only Container IDs

```bash
docker ps -aq
```

### Stop/Kill All Containers

```bash
docker kill $(docker ps -aq)
```

### Remove All Containers

```bash
docker rm $(docker ps -aq)
```

Verify:

```bash
docker ps -a
```

---

# 26. 🗑️ Delete Docker Images

### List Image IDs

```bash
docker images -q
```

### Remove All Images

```bash
docker rmi -f $(docker images -q)
```

Verify:

```bash
docker images
```

> ⚠️ Be careful with bulk cleanup commands because they remove multiple containers/images.

---

# 27. 📄 Dockerfile

A **Dockerfile** is a text file containing instructions used to automatically build a Docker image.

Instead of manually:

```text
Create Container
      ↓
Install Software
      ↓
Configure Software
      ↓
Create Image
```

we write instructions in a Dockerfile:

```text
Dockerfile
     ↓
docker build
     ↓
Image
     ↓
Container
```

---

# 28. ⭐ Why Use Dockerfile?

Dockerfile provides:

* Automation
* Consistency
* Reproducibility
* Version control
* Faster image creation
* Easy CI/CD integration

---

# 29. 🧩 Dockerfile Instructions

## FROM

Defines the base image.

```dockerfile
FROM ubuntu
```

Example:

```dockerfile
FROM amazonlinux
```

---

## RUN

Runs commands while the Docker image is being built.

```dockerfile
RUN apt update
```

Example:

```dockerfile
RUN apt install nginx -y
```

### Important

`RUN` executes during **image build time**.

---

## CMD

Defines the default command executed when a container starts.

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

### Important

`CMD` runs at **container runtime**.

---

## ENTRYPOINT

Defines the main executable for the container.

```dockerfile
ENTRYPOINT ["python3"]
```

`ENTRYPOINT` generally has higher precedence than `CMD`.

---

## COPY

Copies files from the build context into the image.

```dockerfile
COPY index.html /var/www/html/
```

---

## ADD

Adds files or directories to the image.

```dockerfile
ADD application.tar.gz /opt/
```

`COPY` is generally preferred when you simply need to copy local files.

---

## WORKDIR

Sets the working directory.

```dockerfile
WORKDIR /app
```

---

## LABEL

Adds metadata to an image.

```dockerfile
LABEL maintainer="DevOps"
```

---

## ENV

Sets environment variables inside the image/container.

```dockerfile
ENV APP_ENV=production
```

---

## ARG

Defines build-time variables.

```dockerfile
ARG VERSION=1.0
```

Build:

```bash
docker build --build-arg VERSION=2.0 .
```

### ENV vs ARG

```text
ARG
 ↓
Build Time

ENV
 ↓
Container Runtime
```

---

## EXPOSE

Documents the port on which the application listens.

```dockerfile
EXPOSE 8080
```

### Important

`EXPOSE` **does not publish the port** to the host.

To publish a port:

```bash
docker run -p 8080:8080 myimage
```

---

# 30. 🚫 `.dockerignore`

`.dockerignore` tells Docker which files/directories should not be sent as part of the build context.

Example:

```text
.git
.gitignore
node_modules
logs
*.log
```

### Benefits

* Reduces build context size
* Improves build performance
* Prevents unnecessary files from being copied
* Helps avoid accidentally including sensitive/unwanted files

---

# 31. 🔄 Docker Build Process

The overall process:

```text
             Dockerfile
                  │
                  ↓
            docker build
                  │
                  ↓
            Docker Image
                  │
                  ↓
             docker run
                  │
                  ↓
            Container
```

---

# 32. 🧠 Important Docker Concepts

| Concept         | Meaning                                  |
| --------------- | ---------------------------------------- |
| Docker          | Containerization platform                |
| Image           | Read-only blueprint/template             |
| Container       | Running instance of an image             |
| Dockerfile      | Instructions to build an image           |
| Docker Hub      | Public registry for Docker images        |
| `docker pull`   | Download an image                        |
| `docker run`    | Create and start a container             |
| `docker ps`     | Show running containers                  |
| `docker exec`   | Run a command inside a running container |
| `docker attach` | Attach to the main container process     |
| `docker commit` | Create an image from a container         |
| `docker build`  | Build an image from Dockerfile           |
| `docker stop`   | Gracefully stop container                |
| `docker kill`   | Forcefully stop container                |
| `docker rm`     | Remove container                         |
| `docker rmi`    | Remove image                             |

---

# 33. 🎯 Docker Learning Summary

### What I Learned

* Docker installation on Amazon Linux 2023
* Docker images
* Docker containers
* Docker Hub
* Pulling images
* Creating containers
* Interactive mode
* Detached mode
* `docker ps`
* `docker images`
* `docker attach`
* `docker exec`
* Container lifecycle
* Starting and stopping containers
* Restarting containers
* Pausing and unpausing containers
* Container logs
* Container inspection
* Container resource monitoring
* Copying files between host and container
* Saving and loading Docker images
* Docker image layers
* `docker commit`
* Creating custom images
* Docker cleanup
* Dockerfile
* Dockerfile instructions
* `.dockerignore`
* Docker image build process
* `ARG` vs `ENV`
* `RUN` vs `CMD`
* `CMD` vs `ENTRYPOINT`
* `STOP` vs `KILL`
* `ATTACH` vs `EXEC`

---

# 🚀 Final Docker Architecture

```text
                    Docker Host
                         │
          ┌──────────────┴──────────────┐
          │                             │
     Docker Images                Docker Containers
          │                             │
     ┌────┴────┐                 ┌─────┴─────┐
     │         │                 │           │
   Ubuntu  Amazon Linux       App Container  DB Container
     │         │
     └────┬────┘
          │
     Dockerfile
          │
     docker build
          │
     Custom Image
```

---

# 📚 Docker Command Cheat Sheet

```bash
# Docker version
docker -v

# Images
docker images
docker pull ubuntu
docker history ubuntu
docker image prune

# Containers
docker ps
docker ps -a
docker run -it --name ubuntucontainer ubuntu
docker create -it ubuntu

# Access containers
docker attach <container>
docker exec -it <container> /bin/bash

# Container lifecycle
docker start <container>
docker stop <container>
docker restart <container>
docker pause <container>
docker unpause <container>
docker kill <container>

# Container information
docker inspect <container>
docker logs <container>
docker top <container>
docker stats <container>
docker diff <container>

# Container management
docker rename <old-name> <new-name>
docker rm <container>
docker rm -f <container>

# Copy files
docker cp hello.txt <container>:/tmp

# Image creation
docker commit <container> <image>

# Image transfer
docker save ubuntu > ubuntu.tar
docker load -i ubuntu.tar

# Cleanup
docker ps -aq
docker rm $(docker ps -aq)
docker images -q
docker rmi -f $(docker images -q)
```

---

# 🎓 Key Takeaway

> **Docker Image = Blueprint**
> **Docker Container = Running instance of the image**
> **Dockerfile = Instructions to create the image**
> **Docker Build = Process of creating the image**
> **Docker Run = Process of creating and starting the container**

```text
Dockerfile
    ↓
   Build
    ↓
  Image
    ↓
   Run
    ↓
Container
```

This forms the foundation for learning advanced Docker topics such as **Docker networking, volumes, Docker Compose, registries, multi-stage builds, and Docker in CI/CD pipelines**.
