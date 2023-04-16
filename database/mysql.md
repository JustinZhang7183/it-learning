# configuration
```
# allow all address with both ipv4 and ipv6 pattern
bind_address = *
```
# user and permission management
```
# create user
create user 'username'@'host' identified by 'password';
# grant permission
grant permissions on database.table to 'username'@'host';
# flush privileges when modify grant table directly by "insert, update or delete", no need when indirectly like "grant, revoke, set password or rename user"
flush privileges;
```