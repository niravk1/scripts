#Pre-Req for Virtual Box

yum install VirtualBox-5.1-5.1.16_113841_el7-1.x86_64
yum install -y gcc make
yum install -y kernel-uek-devel-`uname -r`
/sbin/vboxconfig

#_______________________________________________________________________________________________________________________________________

# Create Hostonly Network and DHCPSERVER

VBoxManage hostonlyif create # this will create vboxnet0
VBoxManage hostonlyif ipconfig vboxnet0 --ip 172.30.0.1 --netmask 255.255.255.0
vboxmanage dhcpserver add --ifname vboxnet0 --ip 172.30.0.10 --netmask 255.255.255.0 --lowerip 172.30.0.11 --upperip 172.30.0.100 --enable
sleep 1

# Create and Modify VM

vboxmanage createvm --name "cloud1" --ostype "Oracle_64" --register
vboxmanage modifyvm "cloud1" --ostype "Oracle_64" --description "cloud1"
vboxmanage modifyvm "cloud1" --memory 2048 --cpus 1
vboxmanage modifyvm "cloud1" --acpi on
vboxmanage modifyvm "cloud1" --boot1 dvd --boot2 disk --boot3 none --boot4 none
#vboxmanage modifyvm "cloud1" --groups "/o3l_3_test"
#vboxmanage modifyvm "cloud1" --usb on --usbxhci on
vboxmanage modifyvm "cloud1" --usb on --audio none
vboxmanage modifyvm "cloud1" --nic1 bridged --bridgeadapter1 enp10s0
vboxmanage modifyvm "cloud1" --nic2 hostonly --hostonlyadapter1 'vboxnet0'


# Add IDE Controller
vboxmanage storagectl "cloud1" --name "IDE Controller" --add ide
vboxmanage storageattach "cloud1" --storagectl "IDE Controller" --port 0 --device 0 --type dvddrive --medium /home/misc/images/OL7U3.iso

# Create Dynamic VDI disk and attach to VM
vboxmanage storagectl "cloud1" --name "SATA Controller" --add sata --controller IntelAHCI
vboxmanage createhd --filename /home/virtualboxvms/cloud1.vdi --format VDI --size 10000
vboxmanage storageattach "cloud1" --storagectl "SATA Controller" --port 0 --device 0 --type hdd --medium cloud1.vdi

# Add IDE Controller with VBoxGuestAdditions.iso
VBoxManage storageattach "cloud1" --storagectl "IDE Controller" --port 1 --device 0 --type dvddrive --medium /usr/share/virtualbox/VBoxGuestAdditions.iso

# Shared Folder Add
VBoxManage sharedfolder add "cloud1" --name vbshare --hostpath /home/misc/vbshare -automount

# Start / Stop VM Bootup
VBoxManage startvm "cloud1" --type gui

#______________________________________________________________________________________
#VBoxManage startvm "cloud1" --type headless
#VBoxManage controlvm "cloud1" poweroff

# Taking Vanilla Snapshot
#VBoxManage snapshot "cloud1" "cloud1_Vanilla"

# Export and Import
#VBoxManage export "cloud1" --output cloud1_clone.ovf
#VBoxManage import cloud1_clone.ovf
