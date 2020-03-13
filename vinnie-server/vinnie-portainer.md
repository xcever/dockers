

# Docker
<!-- https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04 -->

## Install Docker pre-reqs

`sudo apt install apt-transport-https ca-certificates curl software-properties-common`

## Add docker repo

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

`sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"`

## Install Docker

`sudo apt install docker-ce`

## Check if Docker is running correctly

`sudo systemctl status docker`

## Add user to docker group

`sudo usermod -aG docker ${USER}`

# Docker Compose
<!-- https://github.com/docker/compose -->

```sudo curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose```

`sudo chmod +x /usr/local/bin/docker-compose`

## download compose files from repository
Navigate to home directory `cd ~`

`git clone -b vinnie-server https://github.com/hasosoft/dockers.git`

Navigate to `dockers/vinnie-server`

# Portainer
<!-- https://clouding.io/kb/en-us/articles/360010398219-Install-Portainer-on-Ubuntu-18-04 -->

## Add storage for portainer config

`docker volume create portainer_data`

<!-- https://github.com/MatchbookLab/local-persist -->
## Install local-persistant storage driver 

`curl -fsSL https://raw.githubusercontent.com/MatchbookLab/local-persist/master/scripts/install.sh | sudo bash`



## Create storage volumes for Docker

`docker volume create -d local-persist -o mountpoint=/mnt/media/pictures --name=pictures`

`docker volume create -d local-persist -o mountpoint=/mnt/media/movies --name=movies`

`docker volume create -d local-persist -o mountpoint=/mnt/media/music --name=music`

## Start portainer, make sure it works

`docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer`

Check if it works, set up password. After check the process with `docker ps` and kill it with `docker stop {container name}` and remove it with `docker container rm {container name}`

Navigate to portainer compose folder `cd ~/dockers/vinnie-server/portainer/`

Use compose to run portainer `docker-compose up -d`

# Traefik
<!-- https://techrevelations.de/2019/11/10/nextcloud-and-traefik-v2/ -->
<!-- https://www.smarthomebeginner.com/traefik-reverse-proxy-tutorial-for-docker/ -->
