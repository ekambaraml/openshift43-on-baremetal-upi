

### Storage  pool creation using the disks /dev/sdb, /dev/sdc

Reference: https://linuxacademy.com/hands-on-lab/05a6ab84-4bb1-4bef-8ad7-b89da34a9c09/


 
 

## 1. Create PV with attached disk
  ```
  
  # pvdisplay
  # pvcreate /dev/sdb /dev/sdc
  Device /dev/sdb excluded by a filter.
  Device /dev/sdc excluded by a filter.
  
  ;; If any error or reusing partitioned disc, first Remove the partition and wipe
  # wipefs -a /dev/sdb
  # wipefs -a /dev/sdc
  
  # pvcreate /dev/sdb /dev/sdc
  Physical volume "/dev/sdb" successfully created.
  Physical volume "/dev/sdc" successfully created.

```

## 4. Create Volume Group
```
   # vgcreate node-storage /dev/sdb /dev/sdc
     Volume group "node-storage" successfully created
```   
5. Create a storage pool called oc-storage-pool using the node-storage volume group
   $ virsh pool-define-as oc-storage-pool logical --source-name node-storage --target /dev/sdb /dev/sdc
     Pool oc-storage-pool defined
     
6. Start the storage pool
   $ virsh pool-start oc-storage-pool
   Pool oc-storage-pool started
   
   # virsh pool-autostart oc-storage-pool
   Pool oc-storage-pool marked as autostarted

