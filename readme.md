# Kubernetes cluster with 2 worker nodes create an automated application deployment.

# Tools
- MiniKube : For Kubernetes cluster creation and Management
- Kubectl : For Kubernetes cluster interaction throuch CLI (Management)
- Jenkins : For CI Process
- Ansible: For CD process
- Dependencies: JAVA & Python


## Prerequisites:

#### Install Java
```
sudo apt update  //update the package index
sudo apt install default-jdk  //install open JDK

```
* You may install oracle JDK

#### Install Python3
```
sudo apt install -y python3 && sudo apt install -y python3-pip
```

#### Install Ansible
```
sudo pip3 install ansible
```

#### Install OpenShift
```
sudo pip3 install openshift
```

#### Set Enviornment Variable for global CLI interaction
```
echo "export PATH=$PATH:~/.local/bin" >> ~/.bashrc && . ~/.bashrc
```

#### Install docker
```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88
 sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

#### Install Ansible Role
```
ansible-galaxy install geerlingguy.jenkins
``` 

#### Install docker role 
```
ansible-galaxy install geerlingguy.docker
```

## Steps:

#### Start Jenkins server by playbook
```
ansible-playbook jenkin-playbook.yml
```
* Jenkins will start on default 8080 port. make sure to make it publicly accesible using firewall settings
- Access Jenkin dashboard and install pipeline, docker, CloudBees Docker Build and Publish, GitHub, plugins


#### Create a Kubernetes Cluster with two worker node

- Install Minicube
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube
sudo mkdir -p /usr/local/bin/
sudo install minikube /usr/local/bin/
brew install minikube
```

- Start the cluster
```
minikube start --nodes 3 -p multi-cluster --driver docker --cpus 2 --memory 2000m --disk-size 8000mb
```

- check status
minikube status

#### Connect jenkins to your kubernetes cluster
```
sudo cp ~/.kube/config ~jenkins/.kube/
```

#### Create a jenkin pipeline connecting your git repo

#### Create a jenkin file having 4 steps
- build : Build your docker application
- test : test your application
- publish : Publish to docker hub
- deploy : deploy your application and service to cluster

* Playbook.yml will deploy the app and service in ur cluster sequentially

I have done port forwarding in 0.0.0.0 IP to access it outside

```
kubectl port-forward --address 0.0.0.0 service/techtris-svc 5000:80
```
test-url: http://34.69.213.224:5000/

* I have used POD anti affinity policy to strict one node can have only one pod of the app.

We may change the rules

