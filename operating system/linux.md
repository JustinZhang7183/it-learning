# permission of directories and flies
- check current directories or files
```
ls -l
```
- example
```
drwxr-xr-x 2(numbers of link) owner user-group-owner 4096(size) last-modified-date filename
```
## permission constituent
- first bit: 
    1. d: directory
    2. l: soft link
    3. b: block device
    4. c: char device
    5. s: socket
    6. p: pipe
    7. -: normal file
- first rwx: user's permission
    1. r: read
    2. w: write
    3. x: execute
    4. -: none
- second rwx: group's permission
- third rwx: other's permission
## switch to root
```
# reset the password for root
sudo passwd root
# switch root
su
# switch back
su username
```
## grant root permission for other user
```
sudo chmod u+w /etc/sudoers
sudo vim /etc/sudoers
# add a line below the 'root ALL=(ALL:ALL) ALL'
username ALL=(ALL:ALL) ALL
sudo chmod u-w /etc/sudoers
# then use 'sudo su'
```
# environment variables
## Basis
- check all variables
```
export
```
- check all path
```
echo $PATH
```
- add enviroment
    1. export xx=xx : for current session
    2. vim ~/.bashrc : for current user, need execute source /xxx to affect
    3. vim ~/.bash_profile : for current user, need execute source /xxx to affect
    4. vim /etc/bashrc : for all user, need execute source /xxx to affect
    4. vim /etc/profile : for all user, need execute source /xxx to affect
- add PATH
```
# link origin path
export PATH=$PATH:yourPath
```
## for java
- manage multiple version
```
update-alternatives --config java
```
- designate specific version by environment
```
JAVA_HOME="/xxx/xxx/xxx"
```
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
- replace content
```
# pattern
:[rang]s/{pattern}/{string}/[flags]
# example
:%s/find_content/replacement/g
# if no % means cuttent line
# if no g means replace only first match
```
- move cursor to last line
```
G or shift + g
```
- move cursor to head/tail of line
```
0 and $(shift+4)
```
- clear all content
```
# turn into normal mode, then type following command
Esc -> gg -> dG
```
- copy & past a whole line
```
yy then p
```
- delete current line
```
dd
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
### tar
#### options:
- c : Creates Archive 
- x : Extract the archive 
- f : creates archive with given filename 
- t : displays or lists files in archived file 
- u : archives and adds to an existing archive file 
- v : Displays Verbose Information 
- A : Concatenates the archive files 
- z : zip, tells tar command that creates tar file using gzip 
- j : filter archive tar file using tbzip 
- W : Verify a archive file 
- r : update or add file or directory in already existed .tar file 
### lsof (lists open files)
- no parameter : show all active open files
- -i : show all connection
    1. -i 4/6 : IPv4/6
    2. -i TCP/UDP
    3. -i@ip/ip:port : check connection
    4. -i port
    5. -i service : service name in /etc/service
- -u : show open files by specific user 
# network
## power on check
- error info:
```
A start job is running for wait for network to be configured
```
- reduce the wait time
```
vim /etc/systemd/system/network-online.target.wants/systemd-networkd-wait-online.service
# add following content below the [Service]
TimeoutStartSec=2sec
# reboot
```
## set static ip address
```
# vim /etc/netplan/00-installer-config.yaml
network:
  ethernets:
    eth0:
      dhcp4: false
      addresses: [192.168.1.102/24,xxxx:xxxx:xxxx:xxxx::2/64]
      routes:
        - to: default
          via: 192.168.1.1
        - to: default
          via: xxxx::1
      nameservers:
        addresses: [192.168.1.1,xxxx::1]
  version: 2
# apply
netplan apply
```

