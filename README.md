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
  - [Installing Git](#installing-git)
  - [Installing Docker on Amazon Linux server](#installing-docker-on-amazon-linux-server)
  - [Setup Jenkins](#setup-jenkins)

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
ssh-keygen -t rsa -b 4096
ssh-keygen -t dsa 
ssh-keygen -t ecdsa -b 521
ssh-keygen -t ed25519
```

### Boot and connect to your new VM
The steps vary depending on your chosen VM platform. e.g. VMware: In the Navigator panel, right-click the new virtual machine and choose Edit Settings. for New CD/DVD Drive, choose *seed.iso* File.

### Some of important Linux commands
```sh
#display all interfaces which are currently available, even if down
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

#Linux system shutdown | reboot
sudo systemctl poweroff
sudo reboot

service sshd reload
ssh user3@192.168.80.129
```


## Installing Git

```sh
yum install git -y
```

## Installing Docker on Amazon Linux server

```sh

```

## Setup Jenkins
Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.
