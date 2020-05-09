# My Flow

1. Re-wrote dockerfile to be multistage, built, ran & tested the container:

```
cd /sa-webapp

docker build -t eqmvii/k8stutorial-python-api:1 .

docker run -d -p 5050:5000 eqmvii/k8stutorial-python-api:1
```

Test that it's working by sending a curl:

```
curl --header "Content-Type: application/json" --request POST --data '{"sentence":"I love happy sentences and good people!"}' http://localhost:5050/analyse/sentiment

curl --header "Content-Type: application/json" --request POST --data '{"sentence":"I hate sad angry people and rain!"}' http://localhost:5050/analyse/sentiment
```

2. published to docker:

`docker login`

`docker push eqmvii/k8stutorial-python-api:1`

3. Cleaned up locally:

`docker image rm [any old / unused repo/image:tag combinations]`
`docker image prune`
`docker container prune`

= = = = = = = = = = = = = = = = = = = = = = = =

## Building the Docker Container

```
$ docker build -f Dockerfile -t $DOCKER_USER_ID/sentiment-analysis-logic .
```

## Running the Docker Container

```
$ docker run -d -p 5050:5000 $DOCKER_USER_ID/sentiment-analysis-logic
```

The app is listening by default on port 5000. The 5050 port of the host machine is mapped to the port 5000 of the container.

-p 5050:5000 i.e.

``` -p <hostPort>:<containerPort>```

### Verifying that it works

Execute a POST on endpoint

-> `localhost:5050/analyse/sentiment` or

-> `<docker-machine ip>:5050/analyse/sentiment` Docker-machine ip has to be used if your OS doesn't provide native docker support.

Request body:

```
{
    "sentence": "I hate you!"
}
```

## Pushing to Docker Hub

```
$ docker push $DOCKER_USER_ID/sentiment-analysis-logic
```
