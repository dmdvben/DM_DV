
---

# Creating a new droplet
Follow instructions in the slideset  "1. Introduction"  for creating new droplet.

## Installing Docker

These commands are used for installing Docker on Ubuntu-based systems.
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

## Creating shared volumes

```bash
docker volume create rstudio_data
```

```bash
chmod o+rw /var/lib/docker/volumes/rstudio_data/_data
```

```bash
docker volume create postgres_data
```

```bash
chmod o+rw /var/lib/docker/volumes/postgres_data/_data
```

```bash
docker volume create shiny_app_data
```

```bash
chmod o+rw /var/lib/docker/volumes/shiny_app_data/_data
```

## Creating docker network

```bash
docker network create --driver bridge --attachable --scope local --subnet 10.0.42.0/24 --ip-range 10.0.42.128/25 db_r_shiny
```

## Building images

Create a folder called, e.g. "dockerfiles", in the home directory of your droplet using FileZilla.
Place the two dockerfiles "dockerfile_shiny" and "dockerfile_scheduling" in the "dockerfiles" folder using FileZilla.
Using the droplet terminal, navigate to the "dockerfiles" folder and run

```bash
docker build --tag rstudio:1.0.0 -f dockerfile_scheduling .
```
and

```bash
docker build --tag shiny:1.0.0 -f dockerfile_shiny .
```

## Running containers
Remember to use your own _strong_ passwords.

```bash
docker run -d --network db_r_shiny -p 8787:8787 -e PASSWORD=au_dm_dv_student! --name rstudio -v rstudio_data:/home/rstudio rstudio:1.0.0
```

```bash
docker run -d --network db_r_shiny -p 5432:5432 -e POSTGRES_PASSWORD=au_dm_dv_student! --name postgres -e POSTGRES_USER=postgres -v postgres_data:/var/lib/postgresql/data postgres
```

```bash
docker run -d --network db_r_shiny -p 3838:3838 --name rshiny -v /home/shiny_app:/srv/shiny-server shiny:1.0.0
```


## Start cron
Login to Rstudio (in your rstudio container) from your web browser and go to the Terminal inside Rstudio. Start cron by running

```bash
sudo cron start
```

## Check networks and containers

```bash
docker ps
```

```bash
docker network inspect db_r_shiny
```

```bash
docker ps
```








