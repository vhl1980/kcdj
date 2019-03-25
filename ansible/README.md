# ANSIBLE PROVISIONNING CONFIGURATION

## SET UP ANSIBLE

``` bash
cd $KCDJR_HOME
mkdir kcdjr
cd kcdjr
mkdir -p group_vars/all
mkdir host_vars
touch inventory
vim inventory
# COPY LINES
[manager]
dadm
[worker]
dwk1
dwk2
# END COPY

cd group_vars/all
touch all.yml
vim all.yml
# COPY LINES
---
ansible_become: yes
ansible_become_method: sudo

# PATH LOCAL
kcdjr_home: "/home/ansible/project/kcdjr/ansible"
kcdjr_package: "{{ kcdjr_home }}/package"
# END COPY

cd $KCDJR_HOME
mkdir package
mkdir roles

cd roles
mkdir init_host
cd init_host
mkdir tasks templates
cd tasks
vim ssh-password-less.yml
# copy lines
---
- hosts: all
  tasks:
    - name: Set authorized key taken from file
      authorized_key:
        user: ansible
        state: present
        key: "{{ lookup('file', '/home/ansible/.ssh/id_rsa.pub') }}"
# end copy

# SET UP SSH AND HOSTS
# FOR EACH HOST - EXAMPLE WITH dadm
ssh dadm
sudo su -
vi /etc/hosts
# REPRODUCE LINES BELOW
#127.0.0.1      dadm    dadm
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
#::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

# kcdjr
192.168.0.170 dadm.vhl.com dadm
192.168.0.171 dwk1.vhl.com dwk1
192.168.0.172 dwk2.vhl.com dwk2
# END REPRODUCE

ansible-playbook --ask-pass -i $KCDJR_ANSIBLE/kcdjr $KCDJR_ANSIBLE/roles/init_host/tasks/ssh-password-less.yml

# WELL DONE ANSIBLE ACESS SSH PASSWORD LESS
```
