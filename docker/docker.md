## Instalation of Docker on UBUNTU WSL or AWS Ubuntu Instance


##### Uninstall Old version
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
### Install using the repository

##### Set-up Repository
###### 1.  Update the apt package index and install packages to allow apt to use a repository over HTTPS:
```
 sudo apt-get update

 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
###### 2. Add Docker’s official GPG key:
```
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
###### 3. set up a stable repository
```
 echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
##### Install Docker Engine
###### 1. Update the apt package index, and install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:
```
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io
```
###### 2.  Enable Permission and start docker

```
 sudo service start docker
 sudo chmod 666 /var/run/docker.sock
```
###### 3. verify that docker is working
```
  docker ps
```

##### OUTPUT
```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
#### NB You can now install Kubenetes   
##### more on docker website https://docs.docker.com/engine/install/ubuntu/