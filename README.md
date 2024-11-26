# Amnezia headless installation manual

## Set up amnezia vpn config

```sh
mkdir ~/.config/amnezia
mv awg.conf ~/.config/amnezia

chmod 600 ~/.config/amnezia/awg.conf
```

## Install wire-guard dependencies

### Ubuntu

```sh
sudo apt update
sudo apt install resolvconf
```

## Install wire-guard

From the `$HOME` directory:

```sh
git clone https://github.com/amnezia-vpn/amneziawg-tools.git
cd amneziawg-tools/src
make
sudo make install  # installs awg and awg-quick commands
```

## Check your ip address before turning on vpn

```sh
curl ipconfig.me
```

## Turn on vpn

```sh
sudo awg-quick up ~/.config/amnezia/awg.conf
```

## Check vpn status

```sh
sudo awg
```

## Check your ip address after turning on vpn

```sh
curl ipconfig.me
```

## Install kernel module build dependencies

### Ubuntu

```sh
sudo apt update
sudo apt install gcc-12
```

## Install kernel module

From the `$HOME` directory:

```sh
git clone https://github.com/amnezia-vpn/amneziawg-linux-kernel-module.git
cd amneziawg-linux-kernel-module/src

uname -r  # get kernel version, outputs something like '6.8.0-49-generic'
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v6.x/linux-6.8.1.tar.xz
tar -xJvf linux-6.8.1.tar.xz -C /usr/src

ln -s /usr/src/linux-6.8.1 kernel
make
sudo make install
```
