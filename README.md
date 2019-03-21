# KCDJR
**Stack OLPT** : KAFKA - CASSANDRA - DOCKER - JAVA

The purpose of project is to create stack OLPT by using 

- VAGRANT provisionning VM
- ANSIBLE provisionning configuration VM
- DOCKER provisionning Services 
  - KAFKA 
  - ZOOKEEPER 
  - CASSANDRA 
  - REDIS
  - APPS JAVA

## VAGRANT
[PROVISIONNING CENTOS7 MACHINES](vagrant/README.md)

## ANSIBLE
### create user ansible

``` bash
sudo su -

groupadd -g 9999 ansible
useradd -m -s /bin/bash ansible -u 9999 -g ansible
echo "%ansible ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/ansible
echo -e "ansible\nansible" | (passwd ansible)
cp /etc/ssh/sshd_config /etc/ssh/ori_sshd_config
sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
service sshd restart
```
### set user ansible
Log in with ansible

``` bash
su - l ansible

# Input enter until end
ssh-keygen

cd ~
vim .bashrc
# add end of file
source .KCDJR

touch .KCDJR
# add lines in .KCDJR
#!/bin/bash
# VAR ENV FOR PROJECT KCDJR
export KCDJR_HOME="/home/ansible/project/kcdjr"
export KCDJR_VAGRANT=$KCDJR_HOME/vagrant
export KCDJR_ANSIBLE=$KCDJR_HOME/ansible
```
[PROVISIONNING CENTOS7 CONFIGURATION](ansible/README.md)

## DOCKER



