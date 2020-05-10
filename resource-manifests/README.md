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

## Deploying the Python API


```
kubectl apply -f sa-logic-deployment.yaml

kubectl rollout status deployment sa-logic
```

```
$ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
sa-frontend-ff445c86d-2nwgr   1/1     Running   0          5m
sa-frontend-ff445c86d-gw5n7   1/1     Running   0          5m
sa-logic-5b5df78454-qpdj6     1/1     Running   0          59s
sa-logic-5b5df78454-sg4nf     1/1     Running   0          59s
```


```
kubectl apply -f service-sa-logic.yaml

$ kubectl get svc
NAME             TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes       ClusterIP      10.96.0.1        <none>        443/TCP        4h36m
sa-frontend-lb   LoadBalancer   10.98.232.130    <pending>     80:31411/TCP   34m
sa-logic         ClusterIP      10.106.210.144   <none>        80/TCP         2m26s
```

## Deploying the Java backend

```
$ kubectl apply -f sa-web-app-deployment.yaml --record
deployment.apps/sa-web-app created

$ kubectl rollout status deployment sa-web-app
Waiting for deployment "sa-web-app" rollout to finish: 0 of 2 updated replicas are available...
```

```
$ kubectl apply -f service-sa-web-app-lb.yaml
service/sa-web-app-lb created

$ kubectl get svc
NAME             TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes       ClusterIP      10.96.0.1        <none>        443/TCP        4h39m
sa-frontend-lb   LoadBalancer   10.98.232.130    <pending>     80:31411/TCP   36m
sa-logic         ClusterIP      10.106.210.144   <none>        80/TCP         4m57s
sa-web-app-lb    LoadBalancer   10.99.213.222    <pending>     80:31139/TCP   9s
```

## Wiring up the IP addresses correctly

Problem: the nginx-frontend is trying to hit localhost 80 to find the java load balancer... and it won't find it!

```
minikube service list

|-------------|----------------|--------------|-----------------------------|
|  NAMESPACE  |      NAME      | TARGET PORT  |             URL             |
|-------------|----------------|--------------|-----------------------------|
| default     | kubernetes     | No node port |
| default     | sa-frontend-lb |           80 | http://172.17.227.179:31411 |
| default     | sa-logic       | No node port |
| default     | sa-web-app-lb  |           80 | http://172.17.227.179:31139 |
| kube-system | kube-dns       | No node port |
|-------------|----------------|--------------|-----------------------------|
```

the lb for the webapp backend is: http://172.17.227.179:31139

So we need to re-build our nginx frontend pod to point there

re-build the image, tagged as minikube, created a new deployment, and then fired ze missiles:

```
docker push eqmvii/k8stutorial-nginx-frontend:minikube
```

(note that push is VERY fast since the only changed layer differes by a few hundered megabytes)

```
cd ~/repos/k8s-mastery/resource-manifests/ && kubectl apply -f sa-frontend-deployment-minikube.yaml && kubectl rollout status deploy
ment sa-frontend

```

## Check it out!

minikube service sa-frontend-lb

IT'S ALIVE!
