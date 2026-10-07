## Clone into repo

## Build the Jenkins
```
docker build -t jenkins:latest . 

```

## Create the network Jenkins
```
docker network create jenkins
```

## Run the Container

### Windows
```
docker run --name jenkins --restart=on-failure --detach --network jenkins --env DOCKER_HOST=tcp://docker:2376 --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 --volume jenkins-data:/var/jenkins_home --volume jenkins-docker-certs:/certs/client:ro --publish 8080:8080 --publish 50000:50000 jenkins:latest
```

## Get the Password
```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

## Connect to the Jenkins and create user
```
http://localhost:8080/

Choose "Install suggested plugins", then create the admin user.
```

## Installing docker on jenkins plugin

```
Clouds tab, install a plugin, search docker

Clouds tab,  cloud name  select docker, run the script below first

docker run -d --restart=always --name socat --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock

then docker ps, grabe the IP address of the new container

grab the value of "IPAddress"

then put this value into the "DOCKER HOST URL" in the jenkins FE cloud tab 

tcp://<IPAddress>:2375

then SAVE

```

## Adding docker agent templates
```
Go to clouds then select GEAR ICON of the new cloud you made (it should be named docker as of now)

click "Docker Agents Templates" then "Add Docker Templates"

fill up the label ( should be descriptive, any names  ) : docker-agents

enabled

name ( any names ) : docker-agents

"Docker Image"  :    jenkins/agent:alpine-jdk17

Instance Capacity ( low for testing ) : 2

Remote File System Root  : /home/jenkins 

Then click Apply then run the BUILD

it should be built by the agents


NOTE!


If you have for example python in the code but the current docker image you have does not have python installed. You should make an image where python is installed
then you can use that docker image to the agent and it will run smoothly

```
