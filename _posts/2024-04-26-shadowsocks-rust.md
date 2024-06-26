# Install Shadowsocks-Rust Server and Client

## Debian Server

Update your server's existing software:

```shell
apt update && apt upgrade
```

Install the snap daemon:

```shell
apt install snapd
```

Exit and re-SSH into your server to add `/snap/bin` to your `PATH`.

Install the snap core:

```shell
snap install core
```

Install shadowsocks-rust:

```shell
snap install shadowsocks-rust
```

Generate a 128-bit password expressed in base-64 notation:

```shell
openssl rand -base64 16
```

Create the directory for the ShadowSocks configuration file:

```shell
mkdir -p /var/snap/shadowsocks-rust/common/etc/shadowsocks-rust/
```

Edit the ShadowSocks configuration file:

```shell
vi /var/snap/shadowsocks-rust/common/etc/shadowsocks-rust/config.json
```

Example:

```shell
{
  "server": "::",
  "server_port": 8388,
  "method": "2022-blake3-aes-128-gcm",
  "password": "wt2VOIKrsTX8RWzPGVFp5Q=="
}
```

Save the ShadowSocks configuration file.

Open the server port for TCP input in your firewall.

Start the service:

```shell
snap start --enable shadowsocks-rust.ssserver-daemon
```

Check the service status:

```shell
systemctl status snap.shadowsocks-rust.ssserver-daemon.service
```

## Ubuntu Client

The snap daemon is already installed on most Ubuntu systems.

Install shadowsocks-rust:

```shell
sudo snap install shadowsocks-rust
```

Create the directory for the ShadowSocks configuration file:

```shell
sudo mkdir -p /var/snap/shadowsocks-rust/common/etc/shadowsocks-rust/
```

Edit the ShadowSocks configuration file:

```shell
sudo vi /var/snap/shadowsocks-rust/common/etc/shadowsocks-rust/config.json
```

Example:

```shell
{
  "local_address": "127.0.0.1",
  "local_port": 1080,
  "server": "123.123.123.123",
  "server_port": 8388,
  "method": "2022-blake3-aes-128-gcm",
  "password": "wt2VOIKrsTX8RWzPGVFp5Q=="
}
```
Save the ShadowSocks configuration file.

Either start the local client from the command line, or run as a systemd service.

### Run from command line

Start the local client from the command line:

```shell
snap run shadowsocks-rust.sslocal-daemon -c /var/snap/shadowsocks-rust/common/etc/shadowsocks-rust/config.json
```

Leave the terminal window open with `shadowsocks-rust.sslocal-daemon` running in it.

### Run as systemd service

Override the generated systemd service and configure startup options, as explained in the [README](https://github.com/shadowsocks/shadowsocks-rust):

```shell
sudo systemctl edit snap.shadowsocks-rust.sslocal-daemon.service
```

Start the service:

```shell
sudo snap start --enable shadowsocks-rust.sslocal-daemon
```

Check the service status:

```shell
sudo systemctl status snap.shadowsocks-rust.sslocal-daemon.service
```

### Use the proxy

Either configure Firefox to use the SOCKS v5 proxy server on `127.0.0.1` port `1080`, or configure system-wide networking  to use the SOCKS v5 proxy server on `127.0.0.1` port `1080`.
