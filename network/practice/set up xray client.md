# Download & install
- Download and install
```
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```
# Configuration
- Set two proxy ports including http and socks.
```
{
  # DNS configuration
  "dns": {
    "servers": [
      {
        "address": "1.1.1.1",
        "domains": ["geosite:geolocation-!cn"]
      },
      {
        "address": "223.5.5.5",
        "domains": ["geosite:cn"],
        "expectIPs": ["geoip:cn"]
      },
      {
        "address": "114.114.114.114",
        "domains": ["geosite:cn"]
      },
      "localhost"
    ]
  },
  # routing configuration
  "routing": {
    "domainStrategy": "IPIfNonMatch",
    "rules": [
      {
        "type": "field",
        "domain": ["geosite:category-ads-all"],
        "outboundTag": "block"
      },
      {
        "type": "field",
        "domain": ["geosite:cn"],
        "outboundTag": "direct"
      },
      {
        "type": "field",
        "ip": ["geoip:cn", "geoip:private"],
        "outboundTag": "direct"
      },
      {
        "type": "field",
        "domain": ["geosite:geolocation-!cn"],
        "outboundTag": "proxy"
      },
      {
        "type": "field",
        "ip": ["223.5.5.5"],
        "outboundTag": "direct"
      }
    ]
  },
  "inbounds": [
    {
      "port": 1080,
      "listen": "127.0.0.1",
      "protocol": "socks",
      "settings": {
        "udp": true
      }
    },
    {
	    "port": 10800,
	    "listen": "127.0.0.1",
	    "protocol": "http"
    }
  ],
  "outbounds": [
    {
      "protocol": "vless",
      "settings": {
        "vnext": [
          {
            "address": "domainname", 
            "port": 443, 
            "users": [
              {
                "id": "uuid",
                "encryption": "none",
                "flow": "xtls-rprx-direct",
                "level": 0
              }
            ]
          }
        ]
      },
      "streamSettings": {
      	"network": "tcp",
        "security": "xtls",
        "xtlsSettings": {
          "serverName": "domainname"
        }
      }
    },
    {
      "tag": "direct",
      "protocol": "freedom"
    },
    {
      "tag": "block",
      "protocol": "blackhole"
    }
  ]
}
```
# Utilize it in ubuntu server
### Setting environment variables way
- /etc/profile
```
export http_proxy=http://PROXYHOST:PROXYPORT
export http_proxy=http://PROXYHOST:PROXYPORT
source /etc/profile
```
### Using proxy tool - proxychains
- install: apt isntall -y proxychains
- config: /etc/proxychains.conf
```
# add this line in [ProxyList]
socks5 127.0.0.1 10800
```
- input proxychains before the command which you want it to using proxy
```
proxychains git clone ....
```