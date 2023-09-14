# Docker lecture supplement
This document provides the terminal commands for the steps and assignments from the Docker lectures. 
Explanations of the Docker commands and corresponding options can be found in the lecture slides or in the file [Basic Docker Commands](Basic%20Docker%20Commands.md).


## Table of Contents
1. Installing Docker
2. Create shared volumes
3. Run Rstudio container
4. Verify persistent storage
5. Entering a container
6. Get Shiny container running
7. Build a network
8. Run Postgresql container and connect to the network
9. Inspect the network
10. Inspecting images
11. Dockerfiles
12. tar files
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

## Create shared volumes
```bash
docker volume create rstudio_data
```
```bash
docker volume ls
```
```bash
docker volume inspect --format '{{ .Mountpoint }}' $(docker volume ls -q)
```
```bash
chmod o+rw /var/lib/docker/volumes/rstudio_data/_data
```

## Run Rstudio container
```Bash
docker run -d -p 8787:8787 -e PASSWORD=MyPassword --name rstudio -v rstudio_data:/home/rstudio rocker/rstudio
```

## Verify persistent storage
The purpose of this exercise is 1) To create a file in the shared volume, 2) To see that a package installed manually in a container does not persist after the container is stopped, 3) To use FileZilla to locate a file in your Ubuntu directory. 
The commands run in the terminal only stops and remove the rstudio container, and then runs it again.

```bash
docker stop rstudio
```
```bash
docker rm rstudio
```
```bash
docker run -d -p 8787:8787 -e PASSWORD=MyPassword --name rstudio -v rstudio_data:/home/rstudio rocker/rstudio
```
## Entering a container
```bash
docker exec -it rstudio /bin/bash
```
```bash
id rstudio
```
```bash
exit
```
## Get Shiny container running
```bash
docker pull rocker/shiny
```
```bash
docker run -d -p 3838:3838 --name rshiny -v shiny_app_data:/srv/shiny-server rocker/shiny
```

## Build a network
```bash
docker network create --driver bridge --attachable --scope local --subnet 10.0.42.0/24 --ip-range 10.0.42.128/25 db_r_shiny
```
```bash
docker network connect db_r_shiny rstudio
```
## Run Postgresql container and connect to the network
```bash
docker run -p 5432:5432 --name postgres -e POSTGRES_PASSWORD=mysecretpassword -e POSTGRES_USER=postgres -d -v postgres_data:/var/lib/postgresql/data postgres
```
```bash
docker network connect db_r_shiny postgres
```
## Inspect the network
```bash
docker network inspect db_r_shiny
```
## Inspecting images
```bash
docker image ls
```
## Dockerfiles
### Dockerfile example
```Dockerfile
# Start from the rocker/rstudio base image which has R and RStudio pre-installed
FROM rocker/rstudio

# The RUN command executes shell commands during the image building process.
RUN apt-get update && apt-get install -y \
	git

# Install R packages using R's built-in 'install.packages' function.
RUN R -e 'install.packages(c("DBI", "RPostgres"))'
```
### Build image from Dockerfile
Assuming you are in the directory where the Dockerfile from the example above (called 'dockerfile_rstudio') is located
```bash
docker image build --tag rstudio:1.0.0 -f dockerfile_rstudio .
```
### Run a container with the new image
```bash
docker ps
```
```bash
docker rm -f rstudio
```
```bash
docker run -d --network db_r_shiny -p 8787:8787 -e PASSWORD=MyPassword --name rstudio -v rstudio_data:/home/rstudio rstudio:1.0.0
```
### Docker II - Assignment I
 
```Dockerfile
# Start from the rocker/rstudio base image which has R and RStudio pre-installed
FROM rocker/rstudio

# The RUN command executes shell commands during the image building process.
RUN apt-get update && apt-get install -y \
	git  \
  libxml2-dev

# Install R packages using R's built-in 'install.packages' function.
RUN R -e 'install.packages(c("DBI", "RPostgres", "tidyverse", "httr", "shiny", "jsonlite"))'
```
Assuming the new Dockerfile is called 'dockerfile2_rstudio' and you are in the directory of that docker file
```bash
docker image build --tag rstudio:1.0.1 -f dockerfile2_rstudio .
```
```bash
docker rm -f rstudio
```
```bash
docker run -d --network db_r_shiny -p 8787:8787 -e PASSWORD=MyPassword --name rstudio -v rstudio_data:/home/rstudio rstudio:1.0.1
```
## tar files

```bash
docker save -o rstudio_1_0_1.tar rstudio:1.0.1
```
```bash
docker rm -f rstudio
```
```bash
docker rmi rstudio:1.0.1
```
```bash
docker load -i rstudio_1_0_1.tar
```
```bash
docker run -d --network db_r_shiny -p 8787:8787 -e PASSWORD=MyPassword --name rstudio -v rstudio_data:/home/rstudio rstudio:1.0.1
```

