# Docker, Docker Compose, and Kubernetes testing app

Forked from https://github.com/rinormaloku/k8s-mastery

To practice docker/k8s concepts

## Run with docker compose

```
docker-compose up -d
```

```
docker-compose ps
```

```
docker-compose down
```

## Run Commands for Docker Containers

Run the nginx/react frontend and python sentiment API:

```
docker run -d -p 80:80 eqmvii/k8stutorial-nginx-frontend:1

docker run -d -p 5050:5000 eqmvii/k8stutorial-python-api:1
```

Now that the python container is running, find its docker ip address

```
docker ps
docker container inspect $PYTHON_CONTAINER_ID
```

Grab "NetworkSettings"/"IPAddress" (mine was ` "IPAddress": "172.17.0.2"`) and use that to run the java app:

```
docker run -d -p 8080:8080 -e SA_LOGIC_API_URL='http:/$THE_IP_YOU_FOUND:5000' eqmvii/k8stutorial-java-backend:1
```

Exmaple: `docker run -d -p 8080:8080 -e SA_LOGIC_API_URL='http://172.17.0.2:5000' eqmvii/k8stutorial-java-backend:1`

= = = = = = = = = = = = = = = = = = = = = = = = =

# Origial Readme

This repository contains the source files needed to follow the series [Kubernetes and everything else](https://rinormaloku.com/series/kubernetes-and-everything-else/) or summarized as an article in [Learn Kubernetes in Under 3 Hours: A Detailed Guide to Orchestrating Containers](https://medium.freecodecamp.org/learn-kubernetes-in-under-3-hours-a-detailed-guide-to-orchestrating-containers-114ff420e882)

To learn more about Kubernetes and other related topics check the following examples with the **Sentiment Analysis** application:

* [Kubernetes Volumes in Practice](https://rinormaloku.com/kubernetes-volumes-in-practice/):
* [Ingress Controller - simplified routing in Kubernetes](https://www.orange-networks.com/blogs/210-ingress-controller-simplified-routing-in-kubernetes)
* [Docker Compose in Practice](https://github.com/rinormaloku/k8s-mastery/tree/docker-compose)
* [Istio around everything else series](https://rinormaloku.com/series/istio-around-everything-else/)
* [Simple CI/CD for Kubernetes with Azure DevOps](https://www.orange-networks.com/blogs/224-azure-devops-ci-cd-pipeline-to-deploy-to-kubernetes)
* Envoy series - to be added!
