



https://access.redhat.com/discussions/3321571#comment-1271131


There are several ways you could set this up, but I would do it like this:

* Hypervisor

enp2s0 and enp3s0 in bond0
bond0.100 with hypervisor management IP
bond0.200 in br200
bond0.300 in br300


* VMs

VLAN200 interface in br200
VLAN300 interface in br300
All the VLAN tagging is done on the hypervisor, the VMs just need to have the correct IP addresses for the correct interface.

Think of the bridge as a Layer 2 network switch. It just learns location of stations based on source MAC, and forwards frames based on dest MAC.

If the hypervisor needs an IP in VLAN200 or VLAN300, then add that hypervisor IP on br200 or br300 respectively.





VLAN -- NIC eth0 -- bridget-en1


cat ifcfg-bridge1-en1
DEVICE="bridge1-en7"
ONBOOT="yes"
TYPE="Bridge"
BOOTPROTO="none"
IPADDR="10.24.0.20"
NETMASK="255.255.255.224"
GATEWAY="10.24.0.1"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
DHCPV6C="no"
STP="on"
DELAY="0.0"


cat ifcfg-bridge2-en2
DEVICE="bridge2-en8"
ONBOOT="yes"
TYPE="Bridge"
BOOTPROTO="none"
IPADDR="192.168.1.1"
NETMASK="255.255.255.0"
GATEWAY="192.168.1.2"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
DHCPV6C="no"
STP="on"
DELAY="0.0"
