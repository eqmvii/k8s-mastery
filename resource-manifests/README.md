# Kubernetes Flow

## Manual: creating pods, services, etc.

Making Pods:

```
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
