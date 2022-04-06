# Installing Helm
 ##### Installing form Script
 ######  Helm now has an installer script that will automatically grab the latest version of Helm and install it locally
 ###### You can fetch that script, and then execute it locally. It's well documented so that you can read through it and understand what it is doing before you run it

 ```
 curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
 chmod 700 get_helm.sh
 ./get_helm.sh
 ```

##### Verify that Helm is working
```
helm version
```
###### Output
```
version.BuildInfo{Version:"v3.8.1", GitCommit:"5cb9af4b1b271d11d7a97a71df3ac337dd94ad37", GitTreeState:"clean", GoVersion:"go1.17.5"}
```
#### NB
###### Next you can install minikube
###### More on helm website https://helm.sh/docs/intro/install/