# Building

This snap should be built on an amd64 system with i386 enabled in APT as to
pull the 32-bits libstdc++; to do this run:
```shell
sudo dpkg --add-architecture i386
sudo apt update
```

To build:
```shell
sudo apt install snapcraft
snapcraft
```

# Installation

Install a locally built snap with:
```shell
sudo snap install --devmode *.snap
```

Or from a store:
```shell
sudo snap install --devmode --beta quortus-epc-lool
```

## Sentinel LDK daemon

In some installations, depending on the licensing scheme, you might need the
Sentinel LDK daemon installed on the target system. This is currently only
available as a .deb, so this is not available for Ubuntu Core but works on
Ubuntu classic 16.04+.

The daemon is available for Linux after accepting a license at:
    [https://sentinelcustomer.gemalto.com/sentineldownloads/?o=Linux]

This is a direct link if you've already accepted the license:
    [ftp://ftp.cis-app.com/pub/hasp/Sentinel_HASP/Runtime_(Drivers)/7.51/Sentinel_LDK_Ubuntu_DEB_Run-time_Installer.tar.gz]

To run the daemon on an `amd64` system, you need the `i386` libc6 package;
here's how to install it:
```shell
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install libc6:i386
```

Install the daemon and check it's running with:
```shell
sudo dpkg -i aksusbd_*.deb
ps ax | egrep '(aksusb|winehasp|hasplmd)'
```

# Setup and operation

Check your system is configured properly:
```shell
quortus-epc-lool.check-sys
```

To achieve basic network setup (mainly forwarding and NAT) run the following
command and add it to your startup scripts:
```shell
sudo quortus-epc-lool.setup-networking
```

Upon installation, the `ran` daemon is launched. It creates a config file under
`/var/snap/quortus-epc-lool/current` which you may edit; restart the service to
pick up changes:
```shell
sudo vi /var/snap/quortus-epc-lool/current/ran.cfg
sudo systemctl restart snap.quortus-epc-lool.ran
```

Provision your license with the `rancli` tool:
```shell
quortus-epc-lool.rancli
```

Check the service is running or follow its output with:
```shell
sudo systemctl status snap.quortus-epc-lool.ran
sudo journalctl -u snap.quortus-epc-lool.ran -f
```

