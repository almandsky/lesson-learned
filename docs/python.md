# Python knowledge

## Create virturn environment

```
pip install virtualenv
python  -m virtualenv env
source env/bin/activate
```


## ubuntu

### install opencv-python

```
sudo apt-get install python-opencv

# for python 3
sudo apt-get install python3-opencv
```

### install mpi4py

```
sudo apt install libopenmpi-dev
pip install mpi4py
```


## Raspberry Pi

### https://www.piwheels.org/

create the `/etc/pip.conf` file:

```
sudo vi /etc/pip.conf
```

Add or replace the following line:

```
[global]
extra-index-url=https://www.piwheels.org/simple
```


