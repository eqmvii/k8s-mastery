# My Flow

1. Added a .dockerignore file

2. Re-wrote dockerfile to be multistage, starting with a node container and to the React app, then copying only the build output to the appropriate folder for the nginx contianer.

```
cd /sa-frontend

docker build -t eqmvii/k8stutorial-nginx-frontend:1 .

docker run -d -p 80:80 eqmvii/k8stutorial-nginx-frontend:1
```

Frontend was serverd at `http://localhost:80/`

3. published to docker:

`docker login`

`docker push eqmvii/k8stutorial-nginx-frontend:1`

4. Cleaned up locally:

`docker image rm [any old / unused repo/image:tag combinations]`
`docker image prune`
`docker container prune`

# Original Instructions

## Starting the Web App Locally
` $ yarn start `

## Building the application
` $ yarn build `

## Building the container
` $ docker build -f Dockerfile -t $DOCKER_USER_ID/sentiment-analysis-frontend . `

## Running the container
` $ docker run -d -p 80:80 $DOCKER_USER_ID/sentiment-analysis-frontend `

## Pushing the container
` $ docker push $DOCKER_USER_ID/sentiment-analysis-frontend `
