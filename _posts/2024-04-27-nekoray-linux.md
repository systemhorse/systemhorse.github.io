# Install nekoray on Ubuntu 24.04

`nekoray` is a Qt-based, cross-platform, GUI client. It supports Shadowsocks, VMess, VLESS, Trojan, TUIC, NaiveProxy, and Hysteria2. The `nekoray` repository is at [https://github.com/MatsuriDayo/nekoray](https://github.com/MatsuriDayo/nekoray), and the `nekoray` documentation is at [https://matsuridayo.github.io](https://matsuridayo.github.io).

This post show how to install `nekoray` on Ubuntu 24.04.

When using Debian-based distributions, it is recommended to use the `.deb` package for installation. 

Determine the most recent release from [https://github.com/MatsuriDayo/nekoray/releases](https://github.com/MatsuriDayo/nekoray/releases).

Open a terminal. Change into your `Downloads` directory:

```
cd ~/Downloads
```

Install `cURL`:

```
sudo apt install curl
```

Download the Debian/Ubuntu package for your chosen release of `nekoray`:

```
curl -L https://github.com/MatsuriDayo/nekoray/releases/download/3.26/nekoray-3.26-2023-12-09-debian-x64.deb -O
```

Install `nekoray` and its prerequisite packages:

```
sudo apt install ./nekoray-3.26-2023-12-09-debian-x64.deb
```

The following packages will be installed:

* `libdouble-conversion3`
* `libmd4c0`
* `libpcre2-16-0`
* `libqt5core5t64` 
* `libqt5dbus5t64`
* `libqt5gui5t64`
* `libqt5network5t64`
* `libqt5qml5`
* `libqt5qmlmodels5`
* `libqt5quick5`
* `libqt5svg5`
* `libqt5waylandclient5`
* `libqt5waylandcompositor5`
* `libqt5widgets5t64`
* `libqt5x11extras5`
* `libxcb-xinerama0`
* `libxcb-xinput0`
* `nekoray`
* `qt5-gtk-platformtheme`
* `qttranslations5-l10n`
* `qtwayland5`

`nekoray` is added to your apps.

![nekoray app icon](/assets/images/nekoray-linux-1.png)

Click the `nekoray` icon to launch `nekoray`.

The first time in, you are asked to choose between `Xray` and `sing-box` as your core. You can change this choice later if you need to.

![nekoray initial choice of core](/assets/images/nekoray-linux-2.png)

The NekoBox management panel appears for the first time.

![nekoray management panel](/assets/images/nekoray-linux-3.png)
