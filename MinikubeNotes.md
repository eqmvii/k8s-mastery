# Running minikube on windows with hyperv

hyperv is used for docker too; it conflicts with VirtualBox.

1. Start cmd/shell as administrator
2. minikube start --driver=hyperv

```
$ kubectl get nodes                                                                                                                    NAME       STATUS   ROLES    AGE   VERSION
minikube   Ready    master   63s   v1.18.0
```
