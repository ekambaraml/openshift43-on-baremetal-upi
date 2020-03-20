# Create Virtual Machines on the Baremetal servers

virsh vol-create-as oc-storage-pool  bootstrap.qcow2 100G
virt-install --name=bootstrap --ram=16000 --vcpus=8 --mac=52:54:00:02:85:01 \
--disk path=/var/lib/uvtool/libvirt/images/bootstrap.qcow2,bus=virtio \
--pxe --noautoconsole --graphics=vnc --hvm \
--network network=ocp,model=virtio --boot hd,network


