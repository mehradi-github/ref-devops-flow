# CI/CD (DevOps flow)
Continuous integration(CI), continuous delivery/deployment(CD) are DevOps practices that aim to speed the software delivery without compromising on quality. By automating as many steps in the process as possible, CI/CD provides rapid feedback builds to shorten the time it takes to release software to users.

<!-- TABLE OF CONTENTS -->
## Table of Contents
- [CI/CD (DevOps flow)](#cicd-devops-flow)
  - [Table of Contents](#table-of-contents)
  - [Run Amazon Linux 2 as a virtual machine on premises](#run-amazon-linux-2-as-a-virtual-machine-on-premises)
    - [Prepare the seed.iso boot image](#prepare-the-seediso-boot-image)
    - [Boot and connect to your new VM](#boot-and-connect-to-your-new-vm)
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

### Boot and connect to your new VM
The steps vary depending on your chosen VM platform. e.g. VMware: In the Navigator panel, right-click the new virtual machine and choose Edit Settings. for New CD/DVD Drive, choose *seed.iso* File.

The following command will get you the private IP address of your interfaces:
```sh
ifconfig -a
```
```sh
ssh user3@192.168.80.129
```



## Setup Jenkins
Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software.
