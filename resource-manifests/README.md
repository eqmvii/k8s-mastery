# Kubernetes Flow

## Manual: creating pods, services, etc.

Making Pods:

```
kubectl create -f sa-frontend-pod.yaml
```

Making a service:

```
kubectl create -f service-sa-frontend-lb.yaml
```

Open the service:

(external ip would otherwise be handled by the cloud provider)

```
minikube service sa-frontend-lb
```

(For me that took me to http://172.17.227.179:32185/ in the browser)

## Deployments

```
kubectl apply -f sa-frontend-deployment.yaml
```

creates if it doesn't exist, -f is the filename. The YAML manifet is what says "I am a deployment"

Now try the new bg color version:

```
kubectl apply -f sa-frontend-deployment-green.yaml --record
```

Hack into a port: kubectl port-forward sa-frontend 88:80


kubectl rollout status deployment sa-frontend

Watch the deployment happen:

```
kubectl rollout status deployment sa-frontend
```

combo platter:

```
kubectl apply -f sa-frontend-deployment.yaml --record && kubectl rollout status deployment sa-frontend
```

Open the load balancer to check:

```
minikube service sa-frontend-lb
```


## Rollbacks and history


```
kubectl rollout history deployment sa-frontend

deployment.apps/sa-frontend
REVISION  CHANGE-CAUSE
5         kubectl.exe apply --filename=sa-frontend-deployment.yaml --record=true
6         kubectl.exe apply --filename=sa-frontend-deployment-green.yaml --record=true
```

```
$ kubectl rollout undo deployment sa-frontend --to-revision=5
deployment.apps/sa-frontend rolled back
```

Easy Money.

## Deploying the other services
