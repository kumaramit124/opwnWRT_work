# opwnWRT_work
# Clone opwnwrt latest source from the openwrt github
  git clone https://github.com/openwrt/openwrt.git

# Quick Start to build the openwrt source
  - Go to openwrt directory 
    cd openwrt
  - Update and install the feeds
    ./scripts/feeds update -a  # to obtain all the latest package definitions defined in feeds.conf
    ./scripts/feeds install -a # to install symlinks for all obtained packages into package/feeds/

# Configure target and options
  make menuconfig  # to select your preferred configuration for the toolchain
  E.g. Target System->Select->x86 and then exit.
  Note: From menu Select the Image type to build - VirtualBox (vdi) to get the direct vdi image format.  

# Build firmware image
  make    # to build your firmware
  After make the image will be under "./bin/targets/<x86>/"

# To install the toolchain run
  make toolchain/install # to install toolchain to compile the custom packages

###### To install the openwrt in VM (virtualBox) ###### 

# Prerequisites
  Download and install VirtualBox on host machine.

# Convert the openwrt build fw image openwrt.img to VirtualBox drive image.
  Go to the fw build output path - targerts/x86/64

# Uncompress the gziped image
  gzip -d openwrt-*x86-64-combined*.img.gz
  VBoxManage convertfromraw --format VDI openwrt-*x86-64-combined*.img openwrt.vdi

# Enlarge the image to a useful size (size is in MB)
  VBoxManage modifymedium openwrt.vdi --resize 128

  OR directly you can build the vdi (virtualBox) image from the openwrt, refer the build section.

###### VM Setup in VirtualBox ######

# Netwrok setup 
  eth0 VirtualBox as Host-only Adapter on adapter vboxnet0.
  eth1 VirtualBox as NAT.
  eth2 VirtualBox as Bridged Adapter.

###### After Boots up the openwrt VM, set up the netwrok on target ######

# Boot into your Virtual Machine
# From the console - Display the current network configuration
  root@openwrt:~# uci show network
  Note: that the default LAN address of 192.168.1.1 is present on first boot.

# Edit the network configuration to allow SSH access by writing these commands and pressing enter:
  a) uci set network.lan.ipaddr='192.168.56.2' 
  b) uci commit
  c) reboot

# Create custom package, build, install, and test on openwrt vm.
  Create a simple helloworld package
  1. Create directory under <pwd>/helloworld;cd hellowworld;touch helloworld.c
  2. Compile, link and test using gcc.
  3. run ./helloworld # it works.

# Create package from your App
  1. cd <pwd>; mkdir -p mypackages/examples/helloworld 
  2. cd home/buildbot/mypackages/examples/helloworld; touch Makefile
  3. Go to openwrt source and create - touch feeds.conf
  4. In feeds.conf add - src-link mypackages /home/<user>/mypackages
  5. Update and installing feeds
     ./scripts/feeds update mypackages
     ./scripts/feeds install -a -p mypackages
  6. From "make manuconfig" select the "Example" sub-menu - helloworld
  7. Compile custom package - make package/feeds/helloworld/compile
  8. The output will be - bin/packages/<arch>/mypackages
  9. scp the build .ipk to openwrt vm and install using "opkg install" command

# Install packages directory from openwrt VM
  Run the openwrt x86 VM
  From the console run  - opkg updates, opkg install <package name>
  Once install you can configure the config file if it has any.
  Start using it. 

# Create fcgi sample server package
  Create the fcgi hellow world application package (.ipk) 
  Copy the package on x86 virtual router 
  Install the package on virtual router

# Configure the lighttpd server config
  Update the lighttpd config to handle the client request
  Tested the basic phpinfo page to access
  Also tested fcgi webapp, which respond with hellow world.  

# Referance
  https://openwrt.org/docs/guide-user/virtualization/virtualbox-vm
   
# To install the packages refer
  https://openwrt.org/docs/guide-user/services/webserver/lighttpd

# Custom package build referance
  https://openwrt.org/docs/guide-developer/helloworld/start  
