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

