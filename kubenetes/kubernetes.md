# Install kubectl on Linux 

#### Install kubectl binary with curl on Linux
###### 1. Download the latest release with the command
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
Note
```
###### 2. Validate the binary (optional)
###### Download the kubectl checksum file:
```
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
```
###### Validate the kubectl binary against the checksum file

```
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
```

###### OUTPUT
```
kubectl: ok
```
###### 3. Install Kubectl
```
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

### NB
###### If you do not have root access on the target system, you can still install kubectl to the ~/.local/bin directory

```
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
```
###### Test to ensure the version you installed is up-to-date:
```
kubectl version --client
```

#### NB
###### Next you can install helm if you want to use Helm chart
###### More on k8 website https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/