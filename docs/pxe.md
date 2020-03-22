https://www.tecmint.com/install-pxe-network-boot-server-in-centos-7/
This will explain how you can install and configure a PXE Server on RHEL/CentOS 7 x64-bit with mirrored local installation repositories, sources provided by CentOS 7 DVD ISO image, with the help of DNSMASQ Server.

Which provides DNS and DHCP services, Syslinux package which provides bootloaders for network booting, TFTP-Server, which makes bootable images available to be downloaded via network using Trivial File Transfer Protocol (TFTP) and VSFTPD Server which will host the local mounted mirrored DVD image – which will act as an official RHEL/CentOS 7 mirror installation repository from where the installer will extract its required packages.

# Step 1: Install and configure DNSMASQ Server

1. No need to remind you that is absolutely demanding that one of your network card interface, in case your server poses more NICs, must be configured with a static IP address from the same IP range that belongs to the network segment that will provide PXE services.

So, after you have configured your static IP Address, updated your system and performed other initial settings, use the following command to install DNSMASQ daemon.

```
# yum install dnsmasq
```

2. DNSMASQ main default configuration file located in /etc directory is self-explanatory but intends to be quite difficult to edit, do to its highly commented explanations.

First make sure you backup this file in case you need to review it later and, then, create a new blank configuration file using your favorite text editor by issuing the following commands.
```
# mv /etc/dnsmasq.conf  /etc/dnsmasq.conf.backup
# nano /etc/dnsmasq.conf
```

3. Now, copy and paste the following configurations on dnsmasq.conf file and assure that you change the below explained statements to match your network settings accordingly.

```
interface=eno16777736,lo
#bind-interfaces
domain=centos7.lan
# DHCP range-leases
dhcp-range= eno16777736,192.168.1.3,192.168.1.253,255.255.255.0,1h
# PXE
dhcp-boot=pxelinux.0,pxeserver,192.168.1.20
# Gateway
dhcp-option=3,192.168.1.1
# DNS
dhcp-option=6,92.168.1.1, 8.8.8.8
server=8.8.4.4
# Broadcast Address
dhcp-option=28,10.0.0.255
# NTP Server
dhcp-option=42,0.0.0.0

pxe-prompt="Press F8 for menu.", 60
pxe-service=x86PC, "Install CentOS 7 from network server 192.168.1.20", pxelinux
enable-tftp
tftp-root=/var/lib/tftpboot
```

The statements that you need to change are follows:

* [ ] interface – Interfaces that the server should listen and provide services.
* [ ] bind-interfaces – Uncomment to bind only on this interface.
* [ ] domain – Replace it with your domain name.
* [ ] dhcp-range – Replace it with IP range defined by your network mask on this segment.
* [ ] dhcp-boot – Replace the IP statement with your interface IP Address.
* [ ] dhcp-option=3,192.168.1.1 – Replace the IP Address with your network segment Gateway.
* [ ] dhcp-option=6,92.168.1.1 – Replace the IP Address with your DNS Server IP – several DNS IPs can be defined.
* [ ] server=8.8.4.4 – Put your DNS forwarders IPs Addresses.
* [ ] dhcp-option=28,10.0.0.255 – Replace the IP Address with network broadcast address –optionally.
* [ ] dhcp-option=42,0.0.0.0 – Put your network time servers – optionally (0.0.0.0 Address is for self-reference).
* [ ] pxe-prompt – Leave it as default – means to hit F8 key for entering menu 60 with seconds wait time..
* [ ] pxe=service – Use x86PC for 32-bit/64-bit architectures and enter a menu description prompt under string quotes. Other 
* [ ] values types can be: PC98, IA64_EFI, Alpha, Arc_x86, Intel_Lean_Client, IA32_EFI, BC_EFI, Xscale_EFI and X86-64_EFI.
* [ ] enable-tftp – Enables the build-in TFTP server.
* [ ] tftp-root – Use /var/lib/tftpboot – the location for all netbooting files.

For other advanced options concerning configuration file feel free to read dnsmasq manual.

## Step 2: Install SYSLINUX Bootloaders

4. After you have edited and saved DNSMASQ main configuration file, go ahead and install Syslinx PXE bootloader package by issuing the following command.

```
# yum install syslinux
```

5. The PXE bootloaders files reside in /usr/share/syslinux absolute system path, so you can check it by listing this path content. This step is optional, but you might need to be aware of this path because on the next step, we will copy of all its content to TFTP Server path.

```
# ls /usr/share/syslinux
```

Step 3: Install TFTP-Server and Populate it with SYSLINUX Bootloaders

6. Now, let’s move to next step and install TFTP-Server and, then, copy all bootloders files provided by Syslinux package from the above listed location to /var/lib/tftpboot path by issuing the following commands.

```
# yum install tftp-server
# cp -r /usr/share/syslinux/* /var/lib/tftpboot

```
