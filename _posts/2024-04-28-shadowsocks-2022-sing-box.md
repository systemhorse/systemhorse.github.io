# Create Shadowsocks 2022 sing-box Server

`sing-box` uses JSON for configuration files. Details of the configuration possibilities are at [https://sing-box.sagernet.org/configuration](https://sing-box.sagernet.org/configuration). Example configurations are at [https://github.com/chika0801/sing-box-examples](https://github.com/chika0801/sing-box-examples).

Install `sing-box` as per the instructions at [https://sing-box.sagernet.org/installation/package-manager](https://sing-box.sagernet.org/installation/package-manager).

Donwload the developer's signing key:

```
curl -fsSL https://sing-box.app/gpg.key -o /etc/apt/keyrings/sagernet.asc
```

If necessary, make the signing key readable by all:

```
chmod a+r /etc/apt/keyrings/sagernet.asc
```

Create an APT sources list:

```
echo "deb [arch=`dpkg --print-architecture` signed-by=/etc/apt/keyrings/sagernet.asc] https://deb.sagernet.org/ * *" | tee /etc/apt/sources.list.d/sagernet.list > /dev/null
```

Refresh package lists:

```
apt update
```

Install sing-box:

```
apt install sing-box
```

Generate a 128-bit key, expressed in base-64 notation:

```
openssl rand -base64 16
```

Edit the configuration file `/etc/sing-box/config.json`:

```
vi /etc/sing-box/config.json
```

Insert contents modeled on the example:

```
{
  "inbounds": [
    {
      "type": "shadowsocks",
      "listen": "::",
      "listen_port": 80,
      "method": "2022-blake3-aes-128-gcm",
      "password": "xocpxYhxV2alp7j/4w28CA==", 
      "multiplex": {
        "enabled": true
      }
    }
  ],
  "outbounds": [
    {
      "type": "direct"
    }
  ]
}
```

Save the configuration file `/etc/sing-box/config.json`.

Port `80` (or whatever port number you choose) must be open for input in your server firewall.

Enable sing-box after every reboot:

```
systemctl enable sing-box
```

Start sing-box now:

```
systemctl start sing-box
```

View server log file:

```
journalctl -u sing-box
```
