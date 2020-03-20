

### Storage  pool creation using the disks /dev/sdb, /dev/sdc
```

Create PV with existing disk
  # pvcreate /dev/sdb /dev/sdc
  Device /dev/sdb excluded by a filter.
  Device /dev/sdc excluded by a filter.
  
Remove the partition and wipe
  # wipefs -a /dev/sdb
  # wipefs -a /dev/sdc
  
Create the PV
  # pvcreate /dev/sdb /dev/sdc
  Physical volume "/dev/sdb" successfully created.
  Physical volume "/dev/sdc" successfully created.
  
2. Create Volume Group

   # vgcreate node-storage /dev/sdb /dev/sdc
     Volume group "node-storage" successfully created
   
3. Create a storage pool called oc-storage-pool using the node-storage volume group
   # virsh pool-define-as oc-storage-pool logical --source-name node-storage --target /dev/sdb /dev/sdc
     Pool oc-storage-pool defined
```
