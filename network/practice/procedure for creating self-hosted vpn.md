# Creating self-hosted vpn using vultr vps, x-ray
### resources
###### vultr vps
- buy a os (debian 10/11)
###### namesilo dns
- buy a cost-effective domain name
### install necessary softwares
```
apt-get install -y openssl cron socat curl unzip vim
```
### apply for a certificat
###### using acme.sh
- download acme.sh
```
curl https://get.acme.sh | sh -s email=my@example.com
source ~/.bashrc
```
- use acme.sh to apply the certificat
```
# change the default ca org
acme.sh --set-default-ca --server letsencrypt
# disable the firewall
ufw disable
# verify
acme.sh --issue -d domainname --standalone -k ec-256
```
- install certificat
```
acme.sh --installcert -d domainname --fullchainpath /etc/ssl/private/domainname.crt --keypath /etc/ssl/private/domainname.key --ecc
chmod 755 /etc/ssl/private/*
chmod 755 /etc/ssl/private
```
- auto upgrade for acme.sh
```
acme.sh --upgrade --auto-upgrade
```
### set up a fake website
###### using nginx and static template website
- install nginx
```
apt update && apt install nginx -y
```
- create a dir for website
```
mkdir -p /var/www/website/html
```
- download a template (https://templated.co/)
```
wget -O web.zip --no-check-certificate url
```
- unzip to dir setted before
```
unzip -o -d /var/www/website/html web.zip
```
- modify nginx.conf
```
vim /etc/nginx/nginx.conf
server {
  listen 80;
  server_name domainname;
  root /var/www/website/html;
  return 301 https://$http_host$request_uri;
}
server {
  listen 127.0.0.1:8080;
  root /var/www/website/html;
  index index.html;
  add_header Strict-Transport-Security "max-age=63072000" always;
}
```
### install xray
- download and install
```
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```
- get an uuid
```
cat /proc/sys/kernel/random/uuid
```
- modify config.json
```
vim /usr/local/etc/xray/config.json
{
    "log": {
        "loglevel": "debug",
        "access": "/var/log/xray/access.log",
        "error": "/var/log/xray/error.log"
    },

    "inbounds": [
        {
            "port": 443,
            "protocol": "vless",
            "settings": {
                "clients": [
                    {
                        "id": "UUID", 
                        "flow": "xtls-rprx-direct",
                        "level": 0,
                        "email": "333@ffff.com"
                    }
                ],
                "decryption": "none",
                "fallbacks": [
                    {
                        "dest": 8080
                    }
                ]
            },
            "streamSettings": {
                "network": "tcp",
                "security": "xtls",
                "xtlsSettings": {
                    "alpn": [
                        "http/1.1"
                    ],
                    "certificates": [
                        {
                            "certificateFile": "/etc/ssl/private/domainname.crt",
                            "keyFile": "/etc/ssl/private/domainname.key"
                        }
                    ]
                }
            }
        }
    ],
       
  "outbounds": [
        {
            "protocol": "freedom"
        }
    ]
}
```
### start and start automatically when power on
```
systemctl start xray
systemctl start nginx
systemctl enable xray
systemctl enable nginx
```
### apply certificat automatically
```
vi /etc/ssl/private/xray-cert-renew.sh
```
```
#!/bin/bash

.acme.sh/acme.sh --install-cert -d a-domainname --ecc --fullchain-file  /etc/ssl/private/domainname.crt --key-file  /etc/ssl/private/domainname.key
echo "Xray Certificates Renewed"
       
chmod +r /etc/ssl/private/domainname.key
echo "Read Permission Granted for Private Key"

sudo systemctl restart xray
echo "Xray Restarted"
```
```
chmod +x /etc/ssl/private/xray-cert-renew.sh
crontab -e
0 1 1 * *   bash /etc/ssl/private/xray-cert-renew.sh
```
### bbr
- update environment variables
```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
```
- effect immediately
```
sysctl -p
```
- check kernel if enable bbr
```
sysctl net.ipv4.tcp_available_congestion_control
```
- it will display follow info if enable：
```
net.ipv4.tcp_available_congestion_control = reno cubic bbr
```
- check if bbr started
```
lsmod | grep bbr
```
- it will display follow info if started：
```
tcp_bbr 20480 21
```
### WARP for network flow to netflix
- check netflix
```
#download check program
wget -O nf https://github.com/sjlleo/netflix-verify/releases/download/v3.1.0/nf_linux_amd64 && chmod +x nf

#excute
./nf

#excute by proxy
./nf -proxy socks5://127.0.0.1:40000
```
- install warp
```
#official tutorial：https://pkg.cloudflareclient.com/install

#install WARP repository GPG and key:
curl https://pkg.cloudflareclient.com/pubkey.gpg | sudo gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg

#add WARP source：
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflare-client.list

#update APT：
apt update

#install WARP：
apt install cloudflare-warp

#register WARP：
warp-cli register

#set mode to proxy mode：
warp-cli set-mode proxy

#connect WARP：
warp-cli connect

#query ip address through proxy:
curl ifconfig.me --proxy socks5://127.0.0.1:40000
```
### modify xray configuration
- outbounds
```
    {
      "tag": "netflix_proxy",
      "protocol": "socks",
      "settings": {
        "servers": [
          {
            "address": "127.0.0.1",
            "port": 40000
          }
        ]
      }
    }
```
- routing.rules
```
  "routing": {
    "rules": [
      {
        "type": "field",
        "outboundTag": "netflix_proxy",
        "domain": [
          "geosite:netflix",
          "geosite:disney"
        ]
      }
    ]
  }
```
