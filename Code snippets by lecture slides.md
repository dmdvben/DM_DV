This file provides the code snippets used in class and is ordered by the slide set and slide number in which they appear. 

# Docker I
## Slide 5

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

## Slide 17
```bash
docker ps
```
```bash
docker ps -a
```
## Slide 18
```bash
docker help
```
## Slide 21
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
## Slide 23
Remember to create your own password in the code below
```bash
docker run -d -p 8787:8787 -e PASSWORD=ThisIs4ThePassword --name rstudio -v rstudio_data:/home/rstudio rocker/rstudio
```
## Slide 24
```bash
docker stop rstudio
```
```bash
docker rm rstudio
```
Remember to create your own password in the code below
```bash
docker run -d -p 8787:8787 -e PASSWORD=ThisIs4ThePassword --name rstudio -v rstudio_data:/home/rstudio rocker/rstudio

The purpose of the steps in this slide are:  1) To create a file in the shared volume, 2) To see that a package installed manually in a container does not persist after the container is stopped, 3) To use FileZilla to locate a file in your Ubuntu directory. 
The commands run in the terminal first stop the rstudio container, then remove the rstudio container, and then runs the container again.

```
## Slide 25
```bash
docker exec -it rstudio /bin/bash
```

```bash
id rstudio
```
```bash
exit
```

## Slide 26 
```bash
docker pull rocker/shiny
```
```bash
docker run -d -p 3838:3838 --name rshiny -v shiny_app_data:/srv/shiny-server rocker/shiny
```

## Slide 32
```bash
docker network ls
```

## Slide 34
```bash
docker network create --driver bridge --attachable --scope local --subnet 10.0.42.0/24 --ip-range 10.0.42.128/25 db_r_shiny
```

## Slide 35
```bash
docker network connect db_r_shiny rstudio
```

## Slide 36
Remember to create your own password in the code below
```bash
docker run -p 5432:5432 --name postgres -e POSTGRES_PASSWORD=ThisIs4ThePassword -e POSTGRES_USER=postgres -d -v postgres_data:/var/lib/postgresql/data postgres
```
```bash
docker network connect db_r_shiny postgres
```

## Slide 37
```bash
docker network inspect db_r_shiny
```


