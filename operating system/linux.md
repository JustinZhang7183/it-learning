# Command line
### System information
- uname
```
# only the system name
uname
# kernel name
uname -s
# network hostname
uname -n
# kernel version
uname -v
# kernel release
uname -r
# hardware name
uname -m
# all info
uname -a
```
- hardware information
```
lshw -short/-html
```
- CPU information
```
lscpu
```
- block device information
```
lsblk -a
```
### Memory
- disable the swap
```
# check if swap is enabled
swapon --show / free -h
# disable swap
swapoff -a
# remove the swap file
rm /swap.img
# remove following line in file fstab, make sure swap file not re-created after reboot
vim /etc/fstab
/swap.img       none    swap    sw      0       0
# check swap is disabled
swapon --show
```
### Network
- firewall
```
ufw status / enable / disable
```
### Vim
- clear all content
```
# turn into normal mode, then type following command
Esc -> gg -> dG
```
- copy & past a whole line
```
yy then p
```
### apt
- source file: /etc/apt/sources.list
- make your update effect
```
source /etc/apt/sources.list
```
- upgrade / update software
```
apt-get upgrade / update
```

