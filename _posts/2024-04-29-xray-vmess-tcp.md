# Install Xray VMess+TCP Server and Client on Linux

## Debian/Ubuntu Server

Generate a UUID by visiting [https://www.uuidgenerator.net](https://www.uuidgenerator.net).

SSH into the server. Update the server:

```shell
apt update && apt upgrade
```

Install or upgrade to the latest Xray-core with user = `root`:

```shell
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install --beta -u root
```

Edit the configuration file:

```shell
vi /usr/local/etc/xray/config.json
```

Use this example as a model to adapt as you wish:

```json
{
  "log": {
    "loglevel": "warning"
  },
  "routing": {
    "domainStrategy": "AsIs",
    "rules": [
      {
        "type": "field",
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "block"
      }
    ]
  },
  "inbounds": [
    {
      "listen": "0.0.0.0",
      "port": 80,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "3796bf0e-c038-47f3-8a0c-49ce6ae18650"
          }
        ]
      },
      "streamSettings": {
        "network": "tcp"
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "tag": "direct"
    },
    {
      "protocol": "blackhole",
      "tag": "block"
    }
  ]
}
```

Open port `tcp/80` (or whatever port number you choose) in your server firewall.

Restart the service with your revised configuration file:

```shell
systemctl restart xray
```

Check the service status:

```shell
systemctl status xray
```

## Debian/Ubuntu Client

Install or upgrade to the latest Xray-core:

```shell
sudo bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install --beta -u root
```

Edit the configuration file:

```shell
sudo vi /usr/local/etc/xray/config.json
```

Use the example as a model to follow. Replace the server IP address and other parameters as you desire:

```json
{
  "log": {
    "loglevel": "warning"
  },
  "routing": {
    "domainStrategy": "AsIs",
    "rules": [
      {
        "type": "field",
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "direct"
      }
    ]
  },
  "inbounds": [
    {
      "listen": "127.0.0.1",
      "port": "1080",
      "protocol": "socks",
      "settings": {
        "auth": "noauth",
        "udp": true,
        "ip": "127.0.0.1"
      }
    },
    {
      "listen": "127.0.0.1",
      "port": "1081",
      "protocol": "http"
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "123.123.123.123",
            "port": 80,
            "users": [
              {
                "id": "3796bf0e-c038-47f3-8a0c-49ce6ae18650"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "tcp"
      },
      "tag": "proxy"
    },
    {
      "protocol": "freedom",
      "tag": "direct"
    }
  ]
}
```

Restart the service:

```shell
sudo systemctl restart xray
```

Check the service status:

```shell
sudo systemctl status xray
```

Either configure Firefox to use the SOCKS v5 proxy server on `127.0.0.1` port `1080`, or configure system-wide networking to use the SOCKS v5 proxy server on `127.0.0.1` port `1080`. In the configuration in this post, you can alternatively use the HTTP proxy server on `127.0.0.1` port `1081`.

