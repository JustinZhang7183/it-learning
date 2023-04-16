# configuration
```
# allow all address with both ipv4 and ipv6 pattern
bind * -::*

# set a password
requirepass xxx

# set base dir
dir xxx

# set rdb filename
dbfilename dump.rdb

# set append filename
appendfilename "appendonly.aof"

# set append frequency
appendfsync everysec(default) / always / no
```