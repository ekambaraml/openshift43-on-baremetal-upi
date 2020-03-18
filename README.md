# Deploying OpenShift 4.3 on a user provisioned baremetal infrastructure on Air-gapped/Disconneted Environment




##  Architecture

![Architecture](https://github.com/ekambaraml/openshift43-on-baremetal-upi/blob/master/images/ocp43-airgap-deployment-architecture-01.png)




## Deployment
Steps to Deploying the OpenShift 4.3 on user provisioned baremetal infrastructure.

### - 1. Cluster sizing
### - 2. Hardware provisioning (Baremetal)
### - 3. Preparing Baremetal server
### - 4. Downloading the Software



### Cluster Specification

| Machine | Count | Operating System | VPC | RAM | Storage | Details
----------|-------| -----------------|-----|-----|---------|--------
Bootstrap | 1 | RHCOS | 4 | 16 GB | 120GB | Install node
Control Plane | 3 | RHCOS | 8 | 16 GB | 120GB | Control Plane / Master node
Compute Node | 3 | RHCOS | 16 | 64 GB | 120GB | Worker Node
Load Balancer | 1 | RHEL | 4 | 8 GB | 120GB | Load Balancer / HAPROXY


### Hardware Provisioning
Provisioning of one or more Baremetal servers with higher CPU/Memory/Storages. No of Baremetal server is depends your max size of your baremetal server available in your infrastructure.

In this example, we are provisioning a single Baremetal server with the following specifications.

#### Baremetal server
- [ ] 60 Core

- [ ] 500 GB RAM

- [ ] 6 x 1 TB Disks

- [ ] 1000 GB Public egress bandwidth

- [ ] 1 Gbps Port Speed

- [ ] RedHat 7.x (64bit)


#### 5. Bastion Host Preparation
The following command steps are run in the Bastion Hosts.

* 5.1 Create Directory structure

```
$ mkdir /repository/ocp43
$ export OCP_BASE="/repository/ocp43"
$ mkdir -p ${OCP_BASE}/{auth,certs,data,downloads}
$ mkdir -p ${OCP_BASE}/downloads/{images,tools,secrets}
```

![BastionHostdirectory](https://github.com/ekambaraml/openshift43-on-baremetal-upi/blob/master/images/bastion-host-directory.png)


* 5.2 Install packages

  During the install setup, we will require the following redhat packages. Install the packages on Bastion with the following   command.

```
  $ yum install -y jq openssl podman p7zip httpd-tools curl wget screen nmap telnet
```


* 5.3 Install OCP 4.3 CLI

  You need OpenShift 4.3 **oc** commandline CLI tool for deploying the 4.3 cluster. You can download the tool from the           Infrastructure Provider page on the RedHat OpenShift Cluster Manager site, navigate to the page for your installation type     and click Download Command-line Tools.
  
  - Save the file to your file system
  
  - Extract the compressed file
  
  - place it in a directory that is on your path
  
  You can test the setup by running the oc command
  
  ```
  $ oc <command>
  ```
  

* 5.4 Create a mirror registry

  **Mirror registry** is created on Bastion Host for installation in a restricted Network. The Bastion host must have access     to the         internet to obtain the OCP 4.3 images from RedHat hosted registry(Quay.io) into this mirror repository. Also   this bastion     host that has access to both internet and to the target Cluster network where the Baremetal server in         provisioned.
  
  During the bootstrapping process of installation, the images must have the same digests no matter which repository they are   pulled from. To ensure that the release payload is identical, you mirror the images to your local repository.


  






## TroubleShooting FAQ
