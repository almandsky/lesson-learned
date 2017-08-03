# npm knowledge

## npm install got killed in digitalocean instance

It was caused by swap file size too small.

To resolve this issue, need to increase the swap file size to 1 Gb.

```sh
# Make all swap off
sudo swapoff -a

# Resize the swapfile
sudo dd if=/dev/zero of=/swapfile bs=1M count=1024

# Make swapfile usable
sudo mkswap /swapfile

# Make swapon again
sudo swapon /swapfile
```


The other way to solve this issue on 14.04 and 16.04:

```sh
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo swapon --show
sudo cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
sudo sysctl vm.swappiness=10
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
sudo sysctl vm.vfs_cache_pressure=50
echo 'vm.vfs_cache_pressure=50' | sudo tee -a /etc/sysctl.conf
```