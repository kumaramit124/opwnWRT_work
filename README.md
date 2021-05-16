# opwnWRT_work
# Clone opwnwrt latest source from the openwrt github
  git clone https://github.com/openwrt/openwrt.git

# Quick Start to build the openwrt source 
  ./scripts/feeds update -a  # to obtain all the latest package definitions defined in feeds.conf

# Run
  ./scripts/feeds install -a # to install symlinks for all obtained packages into package/feeds/

# Run manuconfig to select the configuration
  make menuconfig  # to select your preferred configuration for the toolchain

# Run make to build fw
  make    # to build your firmware

###### To install the openwrt in VM (virtualBox) ###### 

# Prerequisites
  Download and install VM

# Convert the openwrt build fw image openwrt.img to VBox drive image.
  Go to the fw build output path - targerts/x86/64

# Uncompress the gziped image
  gzip -d openwrt-*x86-64-combined*.img.gz
  VBoxManage convertfromraw --format VDI openwrt-*x86-64-combined*.img openwrt.vdi

# Enlarge the image to a useful size (size is in MB)
  VBoxManage modifymedium openwrt.vdi --resize 128

###### VM Setup in VirtualBox ######

# Netwrok setup 
  eth0 VirtualBox as Host-only Adapter on adapter vboxnet0.
  eth1 VirtualBox as NAT.
  eth2 VirtualBox as Bridged Adapter.

###### After Boot openwrt VM, set the netwrok ######

# Boot into your Virtual Machine
# From the console - Display the current network configuration
  root@openwrt:~# uci show network
  Note: that the default LAN address of 192.168.1.1 is present on first boot.

# Edit the network configuration to allow SSH access by writing these commands and pressing enter:
  a) uci set network.lan.ipaddr='192.168.56.2' 
  b) uci commit
  c) reboot

# Referance
  https://openwrt.org/docs/guide-user/virtualization/virtualbox-vm
   
# To install the packages refer
  https://openwrt.org/docs/guide-user/services/webserver/lighttpd
  
