# Deploying OpenShift 4.3 on a user provisioned baremetal infrastructure




##  Architecture


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

## TroubleShooting FAQ
