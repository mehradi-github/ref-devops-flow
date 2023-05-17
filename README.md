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
    - [Installing Docker](#installing-docker)
  - [Installing Docker on Ubuntu 22.04 LTS](#installing-docker-on-ubuntu-2204-lts)
    - [Set up and install Docker Engine from Docker’s apt repository](#set-up-and-install-docker-engine-from-dockers-apt-repository)
    - [Install Docker manually and manage upgrades manually.](#install-docker-manually-and-manage-upgrades-manually)
    - [Docker Hub Quickstart](#docker-hub-quickstart)
    - [Kubernetes Cluster installation using minikube](#kubernetes-cluster-installation-using-minikube)
    - [Kubernetes Cluster installation using kubeadm](#kubernetes-cluster-installation-using-kubeadm)
    - [Installing Helm](#installing-helm)
  - [Installing Jenkins](#installing-jenkins)
  - [Installing Ansible](#installing-ansible)
  - [Installing Skaffold](#installing-skaffold)

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
# hardware info
cat /proc/cpuinfo
lscpu

# Shows single-line descriptions
whatis neofetch

# Displays manual page of command
man neofetch 

neofetch

#How to find the Linux distribution
cat /etc/os-release
hostnamectl
#User List
cat /etc/passwd
#Group list
cat /etc/group

su - USER
whoami
id
 
#CTRL+L
clear
#Grabing an ip automatically from DHCP
dhclient -v

#Display all interfaces which are currently available, even if down
ifconfig -a
#What Is My IP Address?
curl ifconfig.me

mv -t DESTINATION file1 file2 file3
rm -f name*

#Create a user
useradd [options] USERNAME
passwd USERNAME
#Assigning Sudo Rights to a user
usermod -aG wheel USERNAME
groups USERNAME

#Remove a user from a group
sudo gpasswd -d USERNAME wheel

# Compress Files
tar -czvf logs_archive.tar.gz *
# Extract from a compressed file
tar -zxvf logs_archive.tar.gz -C ./log

#The Redirection Operators (>, >>, <)
# re-writing the file
echo "abc" > test.txt 

# append to a file
echo "defg" >> test.txt

(ls *.txt > txt-files.list && cp *.tx ~) && (ls *.rpm > rpm-packages.list && cp *.rpm ~) || echo "Precedence Test!"

# The Ampersand Operator (&): execute that Linux command in the background
gedit &

# The Semicolon Operator (;): execute commands in a defined, sequential order
pwd ; mkdir test ; cd test ; touch file

# The OR Operator (||): execute the command that follows only if the preceding command fails
bad_command || ls

# The Pipe Operator (|): directs the output of the preceding command as input to the succeeding command
cat test | grep -i "makeuseof"

# The AND Operator (&&): execute commands only if the preceding command was successfully executed
pwd && mkdir test && cd test && bad_command && ls

sudo install visudo  -y

sudo lsof /etc/sudoers
kill -15 PID
visudo
/ALL
USERNAME    ALL=(ALL)       ALL
# /etc/security/limits.conf or /etc/security/limits.d/90-nproc.conf
<user>       -          nproc     2048      <<<----[ Only for "<user>" user ]


#scp ~/.ssh/id_rsa.pub USER@<target-server>:/root/.ssh/uploaded_key.pub
#cat ~/.ssh/uploaded_key.pub >> ~/.ssh/authorized_keys
ssh-copy-id -i ~/.ssh/mykey USERNAME@<target-server>
vi /etc/ssh/sshd_config # passwordAthuntication no
service ssh restart
service sshd reload
ssh USERNAME@<target-server>

# wc is used to find out number of lines, word count, byte and characters count in the files specified in the file arguments.
sudo yum list installed | wc -l
sudo yum list installed > my_list.txt
sudo yum list installed | egrep -i <NAME>
sudo yum update


# Show proxy 
printenv | grep -i proxy
unset all_proxy && unset ALL_PROXY
export all_proxy=socks5://127.0.0.1:20170/ && export ALL_PROXY=socks5://127.0.0.1:20170/
export http_proxy=http://127.0.0.1:20171/ && export HTTP_PROXY=http://127.0.0.1:20171/
export https_proxy=http://127.0.0.1:20171/ && export HTTPS_PROXY=http://127.0.0.1:20171/

# Setting Proxy for sudo
sudo visudo -f /etc/sudoers.d/NAME
# Write in above file
# Defaults env_keep += "no_proxy all_proxy NO_PROXY ALL_PROXY"
Defaults env_keep += "no_proxy NO_PROXY http_proxy HTTP_PROXY https_proxy HTTPS_PROXY"
source ~/.bashrc


#Linux system shutdown | reboot
sudo systemctl poweroff
sudo reboot



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
## Installing Docker on Ubuntu 22.04 LTS

Install [Docker Engine](https://docs.docker.com/engine/install/ubuntu/#install-from-a-package) on Ubuntu :

### Set up and install Docker Engine from Docker’s apt repository

```sh
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker run hello-world
```

### Install Docker manually and manage upgrades manually.
```sh
mkdir docker && cd docker
cat <<EOF | tee ./urls.txt >/dev/null
https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/containerd.io_1.6.9-1_amd64.deb
https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/docker-ce_23.0.5-1~ubuntu.22.04~jammy_amd64.deb
https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/docker-ce-cli_23.0.5-1~ubuntu.22.04~jammy_amd64.deb
https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/docker-buildx-plugin_0.10.4-1~ubuntu.22.04~jammy_amd64.deb
https://download.docker.com/linux/ubuntu/dists/jammy/pool/stable/amd64/docker-compose-plugin_2.6.0~ubuntu-jammy_amd64.deb
EOF


wget -i ./urls.txt

sudo dpkg -i ./containerd.io_1.6.9-1_amd64.deb \
  ./docker-ce_23.0.5-1~ubuntu.22.04~jammy_amd64.deb \
  ./docker-ce-cli_23.0.5-1~ubuntu.22.04~jammy_amd64.deb \
  ./docker-buildx-plugin_0.10.4-1~ubuntu.22.04~jammy_amd64.deb \
  ./docker-compose-plugin_2.6.0~ubuntu-jammy_amd64.deb

sudo systemctl start docker
sudo docker run hello-world

# unisatall
sudo apt remove docker-compose-plugin \
  docker-buildx-plugin \
  docker-ce-cli \
  docker-ce \
  containerd.io
# check
 sudo apt list --installed | grep -i docker
 sudo apt list --installed | grep -i containerd 

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

### Kubernetes Cluster installation using minikube
[minikube](https://minikube.sigs.k8s.io/docs/start/) is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes. to install the latest minikube stable release: 
```sh
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

#Start a cluster using the docker driver
minikube start --driver=docker
# minikube config set driver docker
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
### Kubernetes Cluster installation using kubeadm

Kubeadm is a tool built to provide kubeadm init and kubeadm join as best-practice "fast paths" for creating Kubernetes clusters.
kubeadm performs the actions necessary to get a minimum viable cluster up and running. By design, it cares only about bootstrapping, not about provisioning machines.
More details: [**Kubernetes Cluster installation using kubeadm**](https://github.com/mehradi-github/Kubernetes-kubeadm#kubernetes-cluster-installation-using-kubeadm)

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

## Installing Jenkins
[Jenkins](https://www.jenkins.io/doc/book/installing/linux/#red-hat-centos) is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.
More details: [**Installing Jenkins(LTS)**](https://github.com/mehradi-github/devops-jenkins#installing-jenkinslts)


## Installing Ansible
Ansible automates the management of remote systems and controls their desired state. more details [**Automation with Ansible playbooks**](https://github.com/mehradi-github/automation-with-ansible-playbooks#automation-with-ansible-playbooks)


## Installing Skaffold
[Skaffold](https://skaffold.dev/docs/quickstart/) handles the workflow for building, pushing and deploying your application, allowing you to focus on what matters most: writing code.

```sh
# For Linux x86_64 (amd64)
curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/v2.0.0/skaffold-linux-amd64 && \
sudo install skaffold /usr/local/bin/
```



<!-- ## Install Jenkins with Helm v3
[Jenkins](https://www.jenkins.io/doc/book/installing/kubernetes/) is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.
### Configure Helm
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
### Create a persistent volume
Create a volume which is called [jenkins-pv](./src/kubernetes/jenkins-volume.yaml):
```sh
kubectl create namespace jenkins
kubectl apply -f jenkins-volume.yaml
```
### Create a service account
Run the following command to apply [jenkins-sa](./src/kubernetes/jenkins-sa.yaml):
```sh
kubectl apply -f jenkins-sa.yaml
#minikube start --extra-config=apiserver.authorization-mode=RBAC
```
### Install Jenkins
We will deploy Jenkins including the Jenkins Kubernetes plugin. See the [official chart](https://github.com/jenkinsci/helm-charts/tree/main/charts/jenkins) for more details.

Open the [jenkins-values.yaml](https://raw.githubusercontent.com/jenkinsci/helm-charts/main/charts/jenkins/values.yaml) file in your favorite text editor and modify the following:
- nodePort: Because we are using minikube we need to use NodePort as service type. Only cloud providers offer load balancers. We define port 32000 as port.
- storageClass:
   ```yaml
   storageClass: jenkins-pv
   ```
- serviceAccount: the serviceAccount section of the jenkins-values.yaml file should look like this:
   ```yaml
   serviceAccount:
    create: false
  # Service account name is autogenerated by default
  name: jenkins
  annotations: {}
   ```   
   Where `name: jenkins` refers to the serviceAccount created for jenkins.
- We can also define which plugins we want to install on our Jenkins. We use some default plugins like git and the pipeline plugin.


Now you can install Jenkins:

```sh
chart=jenkinsci/jenkins
helm install jenkins -n jenkins -f jenkins-values.yaml $chart
```
1. Get your 'admin' user password by running:
    ```sh
    jsonpath="{.data.jenkins-admin-password}"
    secret=$(kubectl get secret -n jenkins jenkins -o jsonpath=$jsonpath)
    echo $(echo $secret | base64 --decode)
    ```
2. Get the Jenkins URL to visit by running these commands in the same shell:
    ```sh
    jsonpath="{.spec.ports[0].nodePort}"
    NODE_PORT=$(kubectl get -n jenkins -o jsonpath=$jsonpath services jenkins)
    jsonpath="{.items[0].status.addresses[0].address}"
    NODE_IP=$(kubectl get nodes -n jenkins -o jsonpath=$jsonpath)
    echo http://$NODE_IP:$NODE_PORT/login
    ```
3. Login with the password from step 1 and the username: admin -->


<!-- ```sh
kubectl get namespaces
kubectl get all -n jenkins
kubectl get pv

helm list -n jenkins
helm uninstall jenkins -n jenkins


kubectl get pods -n jenkins
kubectl logs <pod_name> -n jenkins
kubectl describe pod <pod_name> -n jenkins

kubectl describe pod NAME
kubectl logs NAME
kubectl delete pod NAME --grace-period=0 --force --namespace NAMESPACE
``` -->
<!-- ## Installing Git

```sh
yum install git -y
```




## Setup Jenkins
Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software. -->
