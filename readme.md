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
