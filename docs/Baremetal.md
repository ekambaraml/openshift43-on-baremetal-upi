# Baremetal Server Provisioning


wget https://mirrors.edge.kernel.org/centos/7.7.1908/isos/x86_64/CentOS-7-x86_64-Minimal-1908.iso
wget https://mirrors.kernel.org/centos/7.4.1708/isos/x86_64/sha256sum.txt
wget https://mirrors.edge.kernel.org/centos/7.7.1908/isos/x86_64/sha256sum.txt

$ virt-install --virt-type=kvm --name centos7 --ram 2048 --vcpus=1 --os-variant=centos7.0 \
--cdrom=/var/lib/libvirt/boot/CentOS-7-x86_64-Minimal-1908.iso --network=bridge=br0,model=virtio --graphics \
vnc --disk path=/var/lib/libvirt/images/centos7.qcow2,size=40,bus=virtio,format=qcow2 \
--boot useserial=on 



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

