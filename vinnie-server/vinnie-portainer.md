

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

<!-- https://github.com/MatchbookLab/local-persist -->
## Install local-persistant storage driver 

`curl -fsSL https://raw.githubusercontent.com/MatchbookLab/local-persist/master/scripts/install.sh | sudo bash`

## Create storage volumes for Docker

`docker volume create -d local-persist -o mountpoint=/mnt/media/pictures --name=pictures`

`docker volume create -d local-persist -o mountpoint=/mnt/media/movies --name=movies`

`docker volume create -d local-persist -o mountpoint=/mnt/media/music --name=music`

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

## Add network for portainer to connect externally

`docker network create web`

## Start portainer, make sure it works

Navigate to portainer compose folder `cd ~/dockers/vinnie-server/portainer/`

Use compose to run portainer `docker-compose up -d`

Navigate to x.x.x.x:9000 in a browser, set up password, connect portainer to 'Local'

# Traefik
<!-- https://techrevelations.de/2019/11/10/nextcloud-and-traefik-v2/ -->
<!-- https://www.smarthomebeginner.com/traefik-reverse-proxy-tutorial-for-docker/ -->

## Add network for traefik

`docker network create traefik_proxy`

Or go to 'Networks' in portainer and add a network called 'traefik_proxy' while leaving the rest on default

## Port forwarding and Domain

**Make sure** port 80 and/or 443 is forwarded to the IP-address of the *machine running the docker containers*.

**Make sure** the domain is pointing to the *external IP of the router*.

## Start Traefik, make sure it works (portainer)

### Via github

Go to 'Stacks' in portainer, click add stack, select 'git repository', 

fill in **name** `vinnie_server_stack`

**repository url** in this case is `https://github.com/hasosoft/dockers/`

**reference** is `refs/head/vinnie-server` since it's on that 'branch'

**compose path** is `vinnie-server/traefik/docker-compose.yml` since it is in that folder.

**authentication** shouldn't be necessary unless it is in a private repository

### Via compose file

Navigate to `cd ~/dockers/vinnie-server/traefik/`

Copy the contest of the `docker-compose.yml`  file (eg. `cat docker-compose.yml`)

Go to 'Stacks' in portainer, click add stack, and paste the contents of the traefik/docker-compose.yml file into the web editor.

<!-- ## Start Traefik, make sure it works (docker-compose fallback)

**don't use this if you set it up with portainer**

Navigate to `cd ~/dockers/vinnie-server/traefik/`

Use compose to run portainer `docker-compose up -d` -->

## Check if traefik works

Navigate to x.x.x.x:8080 in a browser and see if it works, it should show you a dashboard and should display some charts.

If you go to HTTP -> Routers, it should display `whoami` as the service running on the `websecure` entrypoint. You can verify it's running correctly by navigating to the https domain eg. https://traefik.harro.dev and it should show a text based webpage showing information about the webrequest. SSL certificate should be automatically pulled and applied by Traefik.