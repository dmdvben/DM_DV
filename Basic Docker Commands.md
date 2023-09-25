# Docker Commands Overview

This document provides an overview of basic Docker commands. Each command is accompanied by a brief description and a small example. The commands are categorized based on their purpose.

## Table of Contents

1. [Installing Docker](#installing-docker)
2. [Container Management](#container-management)
3. [Volume Management](#volume-management)
4. [Network Management](#network-management)
5. [Images](#images)
6. [.tar files](#.tar-files)
7. [Dockerfile Example for RStudio Setup](#dockerfile-example-for-rStudio-setup)

---

## Installing Docker

These commands are used for installing Docker on Ubuntu-based systems. It is not essential to understand these in detail, we just need them to get Docker up and running.
You can just copy the entire code-block below into the Ubuntu terminal (remember to right click and then select paste - do not use Ctrl+v), and then press enter. 
Answer Y to all prompts, and press Enter when purple screens appear.

```bash
sudo apt install gnome-terminal
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker run hello-world
```

The installation is successful if you get a message beginning with: "Hello from Docker". 

## Docker Commands Overview

### Container Management
---
#### `docker ps`
Lists currently running containers.
```bash
docker ps
```
---
#### `docker ps -a`
Lists all containers, including the stopped and paused ones.
```bash
docker ps -a
```
---
#### `docker help`
Displays help messages for Docker commands.

**General Syntax**
```bash
docker help [COMMAND]
```
**Example**
```bash
docker help ps
```
---
#### `docker run`
Runs a Docker container.
**General Syntax**

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```
**Example**
```bash
docker run -d -p 8787:8787 -e PASSWORD=au_dm_dv_student! --name rstudio -v rstudio_data:/home/rstudio rocker/rstudio
```
---
#### `docker exec`
Executes commands in a running container.

**General Syntax**
```bash
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```
**Example**
```bash
docker exec -it rstudio /bin/bash
```
---
#### `docker rm`

Removes a stopped Docker container.

**General Syntax**
```bash
docker rm [CONTAINER]
```
**Example**
```bash
docker rm rstudio
```
---
#### `docker rm -f`
Forcefully removes a Docker container, even though it is running.

**General Syntax**
```bash
docker rm -f [CONTAINER]
```
**Example**

```bash
docker rm -f rstudio
```
---
#### `docker pull`
Pulls an image from a Docker registry.
```bash
docker pull rocker/shiny
```
### Volume Management
---
#### `docker volume create`
Creates a Docker shared volume.

**General Syntax**
```bash
docker volume create [OPTIONS] [VOLUME]
```
**Example**
```bash
docker volume create rstudio_data
```
---
#### `docker volume ls`
Lists all Docker volumes.
```bash
docker volume ls
```
#### `docker volume inspect`
Inspect the host location of Docker volumes.
```bash
docker volume inspect --format '{{ .Mountpoint }}' $(docker volume ls -q)
```
### File Permission
---
#### `chmod o+rw`
Changes read and write permissions for 'other' users on a file or directory.

**General Syntax**
```bash
chmod o+rw [FILE_OR_DIRECTORY]
```
**Example**
Here we give read and write permission to 'other' users in a shared volume
```bash
chmod o+rw /var/lib/docker/volumes/rstudio_data/_data
```
---
### Network Management
---
#### `docker network ls`
Lists all Docker networks.
```bash
docker network ls
```
---
#### `docker network create`
Creates a Docker network.

**General Syntax**
```bash
docker network create [OPTIONS] NETWORK
```
**Example**
```bash
docker network create --driver bridge --attachable --scope local --subnet 10.0.42.0/24 --ip-range 10.0.42.128/25 db_r_shiny
```
---
#### `docker network connect`
Connects a Docker container to a network.

**General Syntax**
```bash
docker network connect [NETWORK] [CONTAINER]
```
**Example**
```bash
docker network connect db_r_shiny rstudio
```
---
#### `docker network inspect`
Inspects a Docker network.

**General Syntax**
```bash
docker network inspect [NETWORK]
```
**Example**
```bash
docker network inspect db_r_shiny
```
---
### Images
---
#### `docker image ls`
Lists Docker images that are currently stored on the host machine.

**General Syntax**
```bash
docker image ls [OPTIONS]
```
**Example**
```bash
docker image ls
```
---
#### `docker rmi`
Removes one or more Docker images.

**General Syntax**
```bash
docker rmi [OPTIONS] IMAGE:TAG
```
**Example**
```bash
docker rmi rstudio:1.0.0
```
---
### .tar files
---
#### `docker save -o`
Saves a Docker image to a tar archive.

**General Syntax**
```bash
docker save -o [TAR_FILE_NAME] [IMAGE_NAME]:[TAG]
```
**Example**
```bash
docker save -o rstudio_1_0_0.tar rstudio:1.0.0
```
---
#### `docker load -i`
Loads a Docker image from a tar archive.

**General Syntax**
```bash
docker load -i [PATH_TO_TAR_FILE]
```
**Example**
```bash
docker load -i /path/to/your-image-file.tar
```
## Dockerfile Example for RStudio Setup
```Dockerfile
# Start from the rocker/rstudio base image which has R and RStudio pre-installed
FROM rocker/rstudio
# The RUN command executes shell commands during the image building process.
RUN apt-get update && apt-get install -y \
	git
# Install R packages using R's built-in 'install.packages' function.
RUN R -e 'install.packages(c("DBI", "RPostgres"))'
```





