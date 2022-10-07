# CI/CD (DevOps flow)
Continuous integration(CI), continuous delivery/deployment(CD) are DevOps practices that aim to speed the software delivery without compromising on quality. By automating as many steps in the process as possible, CI/CD provides rapid feedback builds to shorten the time it takes to release software to users.

<!-- TABLE OF CONTENTS -->
## Table of Contents
- [CI/CD (DevOps flow)](#cicd-devops-flow)
  - [Table of Contents](#table-of-contents)
  - [Run Amazon Linux 2 as a virtual machine on premises](#run-amazon-linux-2-as-a-virtual-machine-on-premises)
    - [Prepare the seed.iso boot image](#prepare-the-seediso-boot-image)
    - [Boot and connect to your new VM](#boot-and-connect-to-your-new-vm)
    - [Some of important Linux commands](#some-of-important-linux-commands)
  - [Setup Kubernetes (K8s)](#setup-kubernetes-k8s)
    - [Install kubectl binary with curl](#install-kubectl-binary-with-curl)
    - [Installing Docker](#installing-docker)
    - [Docker Hub Quickstart](#docker-hub-quickstart)
    - [Installing minikube](#installing-minikube)
    - [Installing Helm](#installing-helm)
  - [Install Jenkins with Helm v3](#install-jenkins-with-helm-v3)

![DevOps Flow](/public/assets/images/devops-flow.png "Devops Flow")

## Run Amazon Linux 2 as a virtual machine on premises
Use the Amazon Linux 2 virtual machine (VM) images for on-premises development and testing.
[Run Amazon Linux 2 on premises](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-2-virtual-machine.html)

### Prepare the seed.iso boot image
The seed.iso boot image includes the initial configuration information that is needed to boot your new VM, such as the network configuration, host name, and user data.
- meta-data> – This file includes the hostname and static network settings for the VM.
- user-data – This file configures user accounts, and specifies their passwords, key pairs, and access mechanisms.

The key generation utility – [PuTTYgen](https://www.puttygen.com) can create various public-key cryptosystems including Rivest–Shamir–Adleman (RSA), Digital Signature Algorithm (DSA), Elliptic Curve Digital Signature Algorithm (ECDSA), and Edwards-curve Digital Signature Algorithm (EdDSA) keys.Although PuTTYgen collects keys in its native file format i.e. **.ppk** files, the keys can easily be converted to any file format.

```sh
ssh-keygen -f <filename> -t rsa -b 4096
ssh-keygen -f <filename> -t dsa 
ssh-keygen -f <filename> -t ecdsa -b 521
ssh-keygen -f <filename> -t ed25519
```

### Boot and connect to your new VM
The steps vary depending on your chosen VM platform. e.g. VMware: In the Navigator panel, right-click the new virtual machine and choose Edit Settings. for New CD/DVD Drive, choose *seed.iso* File.

### Some of important Linux commands
```sh
#How to find the Linux distribution
cat /etc/os-release
hostnamectl

#Display all interfaces which are currently available, even if down
ifconfig -a
#What Is My IP Address?
curl ifconfig.me


#Create a user
useradd [options] USERNAME
passwd USERNAME
#Assigning Sudo Rights to a user
usermod -aG wheel USERNAME
groups USERNAME

#Remove a user from a group
sudo gpasswd -d USERNAME wheel


#scp ~/.ssh/id_rsa.pub USER@<target-server>:/root/.ssh/uploaded_key.pub
#cat ~/.ssh/uploaded_key.pub >> ~/.ssh/authorized_keys
ssh-copy-id -i ~/.ssh/mykey USERNAME@<target-server>
vi /etc/ssh/sshd_config # passwordAthuntication no
service ssh restart
service sshd reload
ssh USERNAME@<target-server>


#Linux system shutdown | reboot
sudo systemctl poweroff
sudo reboot

```


## Setup Kubernetes (K8s)
[Kubernetes](https://kubernetes.io/docs/concepts/overview), also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.


### Install kubectl binary with curl
[Install and Set Up kubectl on Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux)

```sh
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

kubectl version --client --output=yaml  
```
### Installing Docker

```sh
yum install docker -y
docker -v

# start docker services
sudo systemctl enable docker
sudo systemctl start docker
service docker status

useradd dockeradmin
passwd dockeradmin
usermod -aG docker dockeradmin

```
### Docker Hub Quickstart
[Docker Hub](https://docs.docker.com/docker-hub) is a service provided by Docker for finding and sharing container images with your team. It is the world’s largest repository of container images with an array of content sources including container community developers, open source projects and independent software vendors (ISV) building and distributing their code in containers.

```sh
docker pull alpine:latest

mkdir /home/demo && touch /home/demo/Dockerfile 
cd /home/demo
cat > Dockerfile <<EOF 
FROM alpine:latest
CMD echo "Hello world!"
EOF

docker build -t <your_username>/my-hello .
docker image ls

rm -rf /home/demo

docker run <your_username>/my-hello

docker login
docker push <your_username>/my-hello

```

### Installing minikube
[minikube](https://minikube.sigs.k8s.io/docs/start/) is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes. to install the latest minikube stable release: 
```sh
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

minikube start
minikube status
minikube ip

kubectl get po -A
minikube dashboard

# To access the dashboard remotely, run the following command:
minikube dashboard --url=true
kubectl proxy --address='0.0.0.0' --disable-filter=true

minikube pause
minikube unpause
minikube stop


```

### Installing Helm
[Helm](https://helm.sh/docs/) is the package manager for Kubernetes, Helm Charts help you define, install, and upgrade even the most complex Kubernetes application.

This guide shows how to [install the Helm CLI](https://helm.sh/docs/intro/install/). 

```sh
wget https://get.helm.sh/helm-v3.10.0-linux-amd64.tar.gz
tar -zxvf helm-v3.10.0-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm

helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo bitnami
helm repo update              # Make sure we get the latest list of charts

```
## Install Jenkins with Helm v3
[Jenkins](https://www.jenkins.io/doc/book/installing/kubernetes/) is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.

Add the Jenkins repo as follows:

```sh
helm repo add jenkinsci https://charts.jenkins.io
helm repo update
```
The helm charts in the Jenkins repo can be listed with the command:
```sh
helm search repo jenkinsci
```
Minikube configured for hostPath sets the permissions on /data to the root account only. Once the volume is created you will need to manually change the permissions to allow the jenkins account to write its data.
```sh
minikube ssh
sudo chown -R 1000:1000 /data/jenkins-volume
```
create a volume which is called [jenkins-pv](./src/kubernetes/jenkins-volume.yaml):
```sh
kubectl apply -f jenkins-volume.yaml
```


<!-- ## Installing Git

```sh
yum install git -y
```




## Setup Jenkins
Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software. -->
