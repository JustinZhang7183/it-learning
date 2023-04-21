# Command Prompt
## Command Prompt Window Style Setting
- install Microsoft YaHei font [execute file](resources/Microsoft%20YaHei%20Mono.ttf)
- set font
```
1. win + r
2. input regedit， enter
3. locate "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Console\TrueTypeFont"
4. modify the item 936 to "*Microsoft YaHei Mono"
```
- set CodePage
```
1. locate "\HKEY_CURRENT_USER\Console\%SystemRoot%_system32_cmd.exe
2. select the decimal option
3. input 936
```
- modify the Command Prompt Window attribute.
## development
```
# check port usage and kill corresponding task
netstat -ano
netstat -aon|findstr "port"
tasklist|findstr "pid"
taskkill /f /t /im TIM.exe or use the task manager in gui(Graphical user interface)
```
# Virtual Machine
## Hyper-v
### Virtual switch
- internal
### some problems
- virtual machine can not ping host, but host can ping virtual machine.
```
it's caused by windows defender, need to update the inbound rule about ICMPv4-In
```