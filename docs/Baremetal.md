# Baremetal Server Provisioning
https://developers.redhat.com/blog/2016/08/18/setting-up-kvm-on-rhel/






#### Bring down the bridge 
```
# ip link set ocp down
or
# ifconfig ocp down
```

#### Delete the bridge
```
# ip link delete ocp type bridge
```

wget https://mirrors.edge.kernel.org/centos/7.7.1908/isos/x86_64/CentOS-7-x86_64-Minimal-1908.iso
wget https://mirrors.kernel.org/centos/7.4.1708/isos/x86_64/sha256sum.txt
wget https://mirrors.edge.kernel.org/centos/7.7.1908/isos/x86_64/sha256sum.txt
```

## 1. Preparing the Baremetal Server

$ yum update
$ yum install -y net-tools curl nano tree wget jq nfs-common cpu-checker

$ grep -E 'svm|vmx' /proc/cpuinfo
- vmx is for Intel processors
- svm is for AMD processors

$ lscpu | grep Virtualization
Virtualization: VT-x

$ yum install qemu-kvm libvirt libvirt-python libguestfs-tools virt-install

$ systemctl enable libvirtd ; systemctl start libvirtd

$lsmod | grep -i kvm

$ brctl show
$ virsh net-dumpxml default

Setup virtual  network

$ echo "BRIDGE=ocp4" >> /etc/sysconfig/network-scripts/ifcfg-bond0
$ /etc/sysconfig/network-scripts/ifcfg-bond0

DEVICE=bond0
BOOTPROTO=none
ONBOOT=yes
USERCTL=no
BONDING_OPTS="mode=4 miimon=100 downdelay=0 updelay=0 lacp_rate=fast xmit_hash_policy=1"
IPADDR=10.148.168.144
NETMASK=255.255.255.192
NM_CONTROLLED=no
BRIDGE=ocp4

### 1. Setup VM network

This defines a separate network that the VMs will use. It explicitly disables the DNS and DHCP, so that our separate instance of dnsmasq can take over.

The following steps are done in the Baremetal server

### [ ] Create and edit a new file in root's home and add the following contents.
     
     vi net_ocp.xml
     
   
     <network>
        <name>ocp</name>
        <forward mode='nat'/>
        <bridge name='br-ocp' stp='on' delay='0'/>
        <dns enable="no"/> 
        <ip address='192.168.10.1' netmask='255.255.255.0'>
        </ip>
      </network>
    
 ### Start VM network, start it and have it autostart
 
       virsh net-define net_ocp.xml
       virsh net-autostart ocp
       virsh net-start ocp
       
### Restart the KVM
      # systemctl enable libvirtd
      # systemctl start libvirtd
### Verify the KVM Install
      # lsmod | grep -i kvm

### Verify the network status
      # ifconfig  br-ocp


### Install DNSMasq/ipxe/tftp

    # yum install -y dnsmasq

### Install pxe


### Name Server setup
- update /etc/resolv.conf
- update /etc/sysconfig/network-scripts/ifcfg-bond0 & /etc/sysconfig/network-scripts/ifcfg-bond1


### MatchBox Setup
https://coreos.com/matchbox/docs/latest/matchbox.html
cd /var/lib/matchbox/assets 

wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/latest/latest/rhcos-4.3.0-x86_64-installer-initramfs.img
wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/latest/latest/rhcos-4.3.0-x86_64-installer-kernel

wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux-4.3.5.tar.gz
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux.tar.gz
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-install-linux-4.3.5.tar.gz
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-install-linux.tar.gz
wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/latest/latest/rhcos-4.3.0-x86_64-installer.iso
wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/latest/latest/rhcos-4.3.0-x86_64-installer-initramfs.img
wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/latest/latest/rhcos-4.3.0-x86_64-installer-kernel
wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/latest/latest/rhcos-4.3.0-x86_64-installer.iso
wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/latest/latest/rhcos-4.3.0-x86_64-metal.raw.gz
wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/latest/latest/rhcos-4.3.0-x86_64-qemu.qcow2.gz
wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/latest/latest/rhcos-4.3.0-x86_64-vmware.ova

