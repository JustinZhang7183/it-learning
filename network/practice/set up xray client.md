# Download & install
- Download and install
```
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```
# Configuration
- Set two proxy ports including http and socks.
```
{
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