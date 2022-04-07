### Installing Elastic Search Using Helmchart


```
Prerequisites

A Kubernetes cluster (you can create it with minikube) "https://github.com/Tobang1/Elasticstack/blob/main/minikube/minikube.md"

kubectl command-line tool installed (https://github.com/Tobang1/Elasticstack/blob/main/kubenetes/kubernetes.md)

Helm package manager installed (https://github.com/Tobang1/Elasticstack/blob/main/Helm/helm.md)
```

#### Set up Kubernetes Cluster for Elasticsearch

###### 1.  First, start Minikube. A multi-node cluster for Elasticsearch requires significant system resources, so make sure you allocate enough CPUs and memory using the --cpus and --memory options:

```
minikube start --cpus 4 --memory 8192
```
###### 2. Check if your cluster is functioning properly by typing:
```
kubectl cluster-info
```
###### Create a namespase with kubectl 
```
kubectl create namespace Test
```
### Deploy Elasticsearch with Helm

###### 1. To start installing Elasticsearch, add the elastic repository in Helm:
```
helm repo add elastic https://helm.elastic.co
```
###### 2. Now, use the curl command to download the values.yaml file containing configuration information:
```
curl -O https://raw.githubusercontent.com/elastic/helm-charts/master/elasticsearch/examples/minikube/values.yaml
```
###### 3. Use the helm install command and the values.yaml file to install the Elasticsearch helm chart:
```
helm install elasticsearch elastic/elasticsearch -n [namespace] -f ./values.yaml
```
###### The first option is to use the get pods command to check if the cluster members are up:
```
kubectl get pods --namespace=default -l app=elasticsearch-master -w

NB. Once the READY column in the output is entirely populated with 1/1 entries, all the cluster members are up:
```
###### 5. Once you successfully installed Elasticsearch, use the kubectl port-forward command to forward it to port 9200:
```
kubectl port-forward svc/elasticsearch-master 9200
```
##### 6. Check your elastic search in browser
```
Ideally if using vscode ssh agent just add port 9200 to the port forward the open localhost:9200 on your browser. or curl localhost:9200

