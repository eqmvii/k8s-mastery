# My Flow

1. Re-wrote dockerfile to be multistage, built & ran the container

```
cd /sa-webapp

docker build -t eqmvii/k8stutorial-java-backend:1 .

docker run -d -p 8080:8080 -e SA_LOGIC_API_URL='localhost:5000' eqmvii/k8stutorial-java-backend:1
```

(Note that with no python logic container running, this won't do much)

Test that it's working by visiting its health check route: http://localhost:8080/testHealth

2. published to docker:

`docker login`

`docker push eqmvii/k8stutorial-java-backend:1`

3. Cleaned up locally:

`docker image rm [any old / unused repo/image:tag combinations]`
`docker image prune`
`docker container prune`

= = = = = = = = = = = = = = = = = = = = = = = =

# Original Instructions

## Packaging the application
` $ mvn install`

## Running the application
` $ java -jar sentiment-analysis-web-0.0.1-SNAPSHOT.jar --sa.logic.api.url=http://localhost:5000 `

## Building the container
` $ docker build -f Dockerfile -t $DOCKER_USER_ID/sentiment-analysis-web-app . `

## Running the container
```
$ docker run -d -p 8080:8080 -e SA_LOGIC_API_URL='http://<container_ip or docker machine ip>:5000' $DOCKER_USER_ID/sentiment-analysis-web-app
```

#### Native docker support needs the Container IP
CONTAINER_IP: To forward messages to the sa-logic container we need to get  its IP. To do so execute:

` $ docker container list`

Copy the id of sa-logic container and execute:

` $ docker inspect <container_id> `

The Containers IP address is found under the property NetworkSettings.IPAddress, use it in the RUN command.

#### Docker Machine on a VM
Get Docker Machine IP by executing:

` $ docker-machine ip `

Use this one in the command.


## Pushing the container
` $ docker push $DOCKER_USER_ID/sentiment-analysis-web-app `


