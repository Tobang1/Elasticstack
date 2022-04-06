# Installing Minikube on Linux 
###  1. Installing using Binary Download

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
#### 2. Start the cluster

###### From a terminal with administrator access (but not logged in as root), run:
```
minikube start
```
#### 3. Interact with your cluster
``` 
Kubectl get po 
```

#### NB
###### You can now use all resources together, Docker , kubctl, helm and minikube together. now you can move on to install elasticstack if youll be using elastic on cluster
###### More on minikube website https://minikube.sigs.k8s.io/docs/start/