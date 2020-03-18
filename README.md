# openshift43-on-baremetal-upi
OpenShift 4.3 on User Provisioned Baremetal Infrastructure


## Cluster Specification

| Machine | Count | Operating System | VPC | RAM | Storage | Details
----------|-------| -----------------|-----|-----|---------|--------
Bootstrap | 1 | RHCOS | 4 | 16 GB | 120GB | Install node
Control Plane | 3 | RHCOS | 8 | 16 GB | 120GB | Control Plane / Master node
Compute Node | 3 | RHCOS | 16 | 64 GB | 120GB | Worker Node
Load Balancer | 1 | RHEL | 4 | 8 GB | 120GB | Load Balancer / HAPROXY



