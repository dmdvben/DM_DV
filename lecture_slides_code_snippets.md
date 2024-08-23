# Docker I
## Slide 5
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

## Slide 37
```bash
docker network inspect db_r_shiny
```


