# VAGRANT

> BOX "centos/7"

PRE REQUIS 
* INSTALL VIRUALBOX
* INSTALL VAGRANT

## SET HOSTS

vim /etc/hosts

``` bash
# kcdjr
192.168.0.170 dadm.vhl.com dadm
192.168.0.171 dwk1.vhl.com dwk1
192.168.0.172 dwk2.vhl.com dwk2
```

## PROVISIONNING SERVER

``` bash
cd $KCDJR_HOME
mkdir vagrant

sudo vagrant init

# Retrieve Vagrantfile from git here
# ADM
vagrant up dadm

# WORKER
vagrant up dwk1
vagrant up dwk2
```


