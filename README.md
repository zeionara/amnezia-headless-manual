# Amnezia headless manual

## Obtain amezia vpn config

```sh
mkdir ~/.config/amnezia
mv awg.conf ~/.config/amnezia

cat ~/.config/amnezia/awg.conf | grep -ve '^\(J\|S\|H\)' > ~/.config/amnezia/awg-fix.conf

chmod 600 ~/.config/amnezia/awg.conf
chmod 600 ~/.config/amnezia/awg-fix.conf
```

## Install wireguard

### Ubuntu

```sh
sudo apt update
sudo apt install wireguard-tools resolvconf


```

### Any linux

```sh
git clone https://github.com/amnezia-vpn/amneziawg-tools.git
cd amneziawg-tools/src
make
sudo make install  # installs awg and awg-quick commands

sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.23.3.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc

git clone https://github.com/amnezia-vpn/amneziawg-go.git
cd amneziawg-go
make
sudo make install

sudo amneziawg-go awg
```

## Check your ip address before turning on vpn

```sh
curl ipconfig.me
```

## Start vpn

```sh
sudo wg-quick up ~/.config/amnezia/awg.conf
```

## Check vpn status

```sh
sudo wg
```

## Check your ip address after turning on vpn

```sh
curl ipconfig.me
```
