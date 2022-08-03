# Docker

What is containers?
- A way to package application with all the necessary dependencies and configuration.
- Portable artifact,easily shared and moved around.
- Make development and devployment more efficient.

Where do container live?
- Private and Public repository like DokcerHub.

## Set up & Installation:
Update the apt package index, and install the latest version of Docker Engine, containerd, and Docker Compose, or go to the next step to install a specific version:

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

```
if got Error like below:

```bash
Failed to start Docker Application Container Engine
docker.service: Failed with result 'exit-code'
or
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
```

### Solution:

```bash
systemctl start docker

```

 Verify that Docker Engine is installed correctly by running the hello-world image.:

```bash
sudo docker run hello-world

```


## Uninstalled Docker Completely From System (Linux):

### Step 1: To identify what installed package you have.
          
```bash
dpkg -l | grep -i docker

```

### Step 2: Remove All Pacakges.
          
```bash
sudo apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli
sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce  

```

The above commands will not remove images, containers, volumes, or user created configuration files on your host. If you wish to delete all images, containers, and volumes run the following commands:

```bash
sudo rm -rf /var/lib/docker /etc/docker
sudo rm /etc/apparmor.d/docker
sudo groupdel docker
sudo rm -rf /var/run/docker.sock

```
You have removed Docker from the system completely.

## Basic Docker Commands:

#### Pull the image from dockerhub:
```bash
docker pull [image name:tag]

```

#### Run the image:
```bash
docker run [image name]

```
If image is not available locally then this command will first pull image from dockerhub and then run its (pull+run).

#### Run the image with detached mode:
```bash
docker run -d [image name]

```

#### List of the images present locally:
```bash
docker images ls

```

#### List all containes:
```bash
docker ps -a

```
#### Start/Stop containes:
```bash
docker start/stop [container ID]

```
#### Bind Ports of host with containers:
```bash
docker run -p [host port]:[container port] --name [container name] [image name:tag]

```
#### Check containers logs:
```bash
docker logs [container ID]

```

#### Enter into container (get container terminal):
```bash
docker exec -it [container ID] /bin/bash

```

#### Delete image:
```bash
docker rmi [image-id/name]

```
#### Delete container:
```bash
docker rm [container-id/name]

```

#### Create own docker network:
```bash
docker network create [network name]

```
container in the same network can talk with container name (no need of port)
#### List docker network:
```bash
docker network network ls

```
