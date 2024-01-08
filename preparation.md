# Kubernetes cluster

[Overview](README.md) | [Preparation](preparation.md) | [K0s](install_k0s.md) | [K3s](master_node.md)

## Steps

- flash an os image into the micro sd cards
- setup network access
- start the network
- add swap memory

## Flash micro sd with Ubuntu server 20.03 LTS

The steps to flash a micro sd card can be found [here](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#1-overview).

## Edit network-config file

Open the boot partition of the flashed sd card and edit the network config file to allow network access.

```yaml

version: 2
ethernets:
  eth0:
    addresses:
      - 192.168.1.120/24
    gateway4: 192.168.1.1
    nameservers:
      addresses: [192.168.1.120]
    optional: true
wifis:
  wlan0:
    addresses:
      - 192.168.1.120/24
    gateway4: 192.168.1.1
    nameservers:
      addresses: [192.168.1.120]
    optional: true
    access-points:
      tim_hoods_5G:
        password: "amoruccioli"

```

## Boot the pi using the micro sd flashed in the previous step

Here you need to type username and password for ubuntu sever which are
password: ubuntu
user: ubuntu

change the password

## Start the network if it doens't start

If the pi is connected via LAN and the network is not up and running, enable the dhclient eth0

```bash
sudo dhclient eth0
```

## Add swap memory[https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04]

Add swap memory to help the PI perform better.

```bash
// Check if swap is on and free memory
sudo swapon --show
free -h

// create a 1GB swap file (1 or 2 times the physical ram)
sudo fallocate -l 2G /swapfile
ls -lh /swapfile

// make the swap file accessibile only by root user
sudo chmod 600 /swapfile
ls -lh /swapfile

// mark the swap space
sudo mkswap /swapfile

//Enable the swap file
sudo swapon /swapfile

// verify if swap is on
sudo swapon --show
free -h

```

## Next

if want to use K0s - [installation](install_k0s.md)

if using K3s: Chose the PI that needs to be the master node and follow the steps [here](master_node.md)
