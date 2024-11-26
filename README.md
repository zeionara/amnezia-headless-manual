# Amnezia headless installation manual

## Set up amnezia vpn config

```sh
mkdir ~/.config/amnezia
mv awg.conf ~/.config/amnezia

chmod 600 ~/.config/amnezia/awg.conf
```

## Install amneziawg dependencies

### From binary

```sh
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.23.3.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
```

### From source

```sh
git clone https://go.googlesource.com/go goroot
cd goroot
git checkout release-branch.go1.23  # you may need to change the version of go toolkit here
cd src
./all.bash
sudo ln -s ../bin/go /usr/bin/go
```

## Install amneziawg

```
git clone https://github.com/amnezia-vpn/amneziawg-go.git
cd amneziawg-go
make
sudo make install
```

## Install wire-guard dependencies

### Ubuntu

```sh
sudo apt update
sudo apt install resolvconf
```

### Gentoo

```sh
sudo emaint --auto sync
sudo emerge -av net-dns/openresolv
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

sudo modprobe amneziawg
```

## Possible problems

Incomplete linux installation at `/usr/src` which leads to missing headers at `/lib/modules`, which results in the following error:

```
make[1]: *** No rule to make target 'modules'.  Stop.
make: *** [Makefile:94: module] Error 2
```

But the next error is not critical when installing kernel module:

```
  INSTALL /lib/modules/6.1.57-x86_64/extra/amneziawg.ko
  SIGN    /lib/modules/6.1.57-x86_64/extra/amneziawg.ko
At main.c:167:
- SSL error:FFFFFFFF80000002:system library::No such file or directory: ../openssl-3.0.12/crypto/bio/bss_file.c:67
- SSL error:10000080:BIO routines::no such file: ../openssl-3.0.12/crypto/bio/bss_file.c:75
sign-file: ./certs/signing_key.pem
  DEPMOD  /lib/modules/6.1.57-x86_64
Warning: modules_install: missing 'System.map' file. Skipping depmod.
depmod -b "/" -a 6.1.57-gentoo-x86_64
```
