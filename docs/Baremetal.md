# Baremetal Server Provisioning


wget https://mirrors.edge.kernel.org/centos/7.7.1908/isos/x86_64/CentOS-7-x86_64-Minimal-1908.iso
wget https://mirrors.kernel.org/centos/7.4.1708/isos/x86_64/sha256sum.txt
wget https://mirrors.edge.kernel.org/centos/7.7.1908/isos/x86_64/sha256sum.txt

$ virt-install --virt-type=kvm --name centos7 --ram 2048 --vcpus=1 --os-variant=centos7.0 \
--cdrom=/var/lib/libvirt/boot/CentOS-7-x86_64-Minimal-1908.iso --network=bridge=br0,model=virtio --graphics \
vnc --disk path=/var/lib/libvirt/images/centos7.qcow2,size=40,bus=virtio,format=qcow2 \
--boot useserial=on 
