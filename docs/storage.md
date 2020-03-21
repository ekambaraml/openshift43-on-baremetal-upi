

### Storage  pool creation using the disks /dev/sdb

Reference: https://linuxacademy.com/hands-on-lab/05a6ab84-4bb1-4bef-8ad7-b89da34a9c09/


 
 

## 1. Create PV with attached disk
  ```
  
  # pvdisplay
  # pvcreate /dev/sdb 
  Device /dev/sdb excluded by a filter.

  
  ;; If any error or reusing partitioned disc, first Remove the partition and wipe
  # wipefs -a /dev/sdb

  
  # pvcreate /dev/sdb /dev/sdc
  Physical volume "/dev/sdb" successfully created.


```

## 2. Create Volume Group
```
  # vgcreate node-storage /dev/sdb
  Volume group "node-storage" successfully created
```   

## 3.Create a storage pool called oc-storage-pool using the node-storage volume group
```
  # virsh pool-define-as oc-storage-pool logical --source-name node-storage --target /dev/sdb /dev/sdc
  Pool oc-storage-pool define


 ; Start the storage pool
 # virsh pool-start oc-storage-pool
 Pool oc-storage-pool started
   
 # virsh pool-autostart oc-storage-pool
 Pool oc-storage-pool marked as autostarted

