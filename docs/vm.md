# Create Virtual Machines on the Baremetal servers


```
1. Bootstrap node
# virsh vol-create-as oc-storage-pool  bootstrap.qcow2 100G
Vol bootstrap.qcow2 created


# virsh vol-list oc-storage-pool
 Name                 Path                                    
------------------------------------------------------------------------------
 bootstrap.qcow2      /dev/node-storage/bootstrap.qcow2       
 

# virt-install --name=bootstrap --ram=16000 --vcpus=8 --mac=52:54:00:02:85:01 \
--disk path=/dev/node-storage/bootstrap.qcow2,bus=virtio \
--pxe --noautoconsole --graphics=vnc --hvm \
--network network=ocp,model=virtio --boot hd,network

# virsh list
 Id    Name                           State
----------------------------------------------------
 1     bootstrap                      running


2.  Master 1 node

  # virsh vol-create-as oc-storage-pool master1.qcow2 200G
  Vol master1.qcow2 created
  
  
  # virt-install --name=master1 --ram=32768 --vcpus=16 --mac=52:54:00:02:86:01 \
    --disk path=/dev/node-storage/master1.qcow2,bus=virtio \
    --pxe --noautoconsole --graphics=vnc --hvm \
    --network network=ocp,model=virtio --boot hd,network
    
3. Master 2 node
    # virsh vol-create-as oc-storage-pool master2.qcow2 200G
    # virt-install --name=master2 --ram=32768 --vcpus=16 --mac=52:54:00:02:86:02 --disk path=/dev/node-storage/master2.qcow2,bus=virtio --pxe --noautoconsole --graphics=vnc --hvm --network network=ocp,model=virtio --boot hd,network
   
4. Master 3 node
   # virsh vol-create-as oc-storage-pool master3.qcow2 200G
   # virt-install --name=master3 --ram=32768 --vcpus=16 --mac=52:54:00:02:86:03 --disk path=/dev/node-storage/master3.qcow2,bus=virtio --pxe --noautoconsole --graphics=vnc --hvm --network network=ocp,model=virtio --boot hd,network
   
   
5. Worker 1 Node
   # virsh vol-create-as oc-storage-pool worker1.qcow2 200G
   # virt-install --name=worker1 --ram=32768 --vcpus=16 --mac=52:54:00:02:87:01 --disk path=/dev/node-storage/worker1.qcow2,bus=virtio --pxe --noautoconsole --graphics=vnc --hvm --network network=ocp,model=virtio --boot hd,network

6. Worker 2 Node
   # virsh vol-create-as oc-storage-pool worker2.qcow2 200G
   # virt-install --name=worker2 --ram=32768 --vcpus=16 --mac=52:54:00:02:87:02 --disk path=/dev/node-storage/worker2.qcow2,bus=virtio --pxe --noautoconsole --graphics=vnc --hvm --network network=ocp,model=virtio --boot hd,network

7. Worker 3 Node
   # virsh vol-create-as oc-storage-pool worker3.qcow2 200G
   # virt-install --name=worker3 --ram=32768 --vcpus=16 --mac=52:54:00:02:87:03 --disk path=/dev/node-storage/worker3.qcow2,bus=virtio --pxe --noautoconsole --graphics=vnc --hvm --network network=ocp,model=virtio --boot hd,network

8. Check the virutal machine Status

[root@onca libvirt]# virsh list
 Id    Name                           State
----------------------------------------------------
 1     bootstrap                      running
 2     master1                        running
 3     master3                        running
 4     master2                        running
 5     worker1                        running
 6     worker2                        running
 7     worker3                        running
 
 
9. Check the volume status

# virsh vol-list oc-storage-pool
 Name                 Path                                    
------------------------------------------------------------------------------
 bootstrap.qcow2      /dev/node-storage/bootstrap.qcow2       
 master1.qcow2        /dev/node-storage/master1.qcow2         
 master2.qcow2        /dev/node-storage/master2.qcow2         
 master3.qcow2        /dev/node-storage/master3.qcow2         
 worker1.qcow2        /dev/node-storage/worker1.qcow2         
 worker2.qcow2        /dev/node-storage/worker2.qcow2         
 worker3.qcow2        /dev/node-storage/worker3.qcow2  
 

