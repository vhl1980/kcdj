# VAGRANT

> BOX "centos/7"

PRE REQUIS 
* INSTALL VIRUALBOX
* INSTALL VAGRANT

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


