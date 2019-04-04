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

## SET DOCKER

### GET - CHOICE VERSION
``` bash
ssh ansible@dadm

sudo su -

#GET REPOS
yum-config-manager \
 --add-repo \
https://download.docker.com/linux/centos/docker-ce.repo

yum list docker-ce --showduplicates

Available Packages
docker-ce.x86_64                                                                                    18.03.0.ce-1.el7.centos                                                                                    docker-ce-stable
docker-ce.x86_64                                                                                    18.03.1.ce-1.el7.centos                                                                                    docker-ce-stable
docker-ce.x86_64                                                                                    18.06.0.ce-3.el7                                                                                           docker-ce-stable
docker-ce.x86_64                                                                                    18.06.1.ce-3.el7                                                                                           docker-ce-stable
docker-ce.x86_64                                                                                    18.06.2.ce-3.el7                                                                                           docker-ce-stable
docker-ce.x86_64                                                                                    18.06.3.ce-3.el7                                                                                           docker-ce-stable
docker-ce.x86_64                                                                                    3:18.09.0-3.el7                                                                                            docker-ce-stable
docker-ce.x86_64                                                                                    3:18.09.1-3.el7                                                                                            docker-ce-stable
docker-ce.x86_64                                                                                    3:18.09.2-3.el7                                                                                            docker-ce-stable
docker-ce.x86_64                                                                                    3:18.09.3-3.el7                                                                                            docker-ce-stable
docker-ce.x86_64                                                                                    3:18.09.4-3.el7                                                                                            docker-ce-stable

yum list docker-ce-cli --showduplicates
Available Packages
docker-ce-cli.x86_64                            1:18.09.0-3.el7                            docker-ce-stable
docker-ce-cli.x86_64                            1:18.09.1-3.el7                            docker-ce-stable
docker-ce-cli.x86_64                            1:18.09.2-3.el7                            docker-ce-stable
docker-ce-cli.x86_64                            1:18.09.3-3.el7                            docker-ce-stable
docker-ce-cli.x86_64                            1:18.09.4-3.el7                            docker-ce-stable

yum list containerd.io --showduplicates
Available Packages
containerd.io.x86_64                         1.2.0-1.2.beta.2.el7                          docker-ce-stable
containerd.io.x86_64                         1.2.0-2.0.rc.0.1.el7                          docker-ce-stable
containerd.io.x86_64                         1.2.0-2.2.rc.2.1.el7                          docker-ce-stable
containerd.io.x86_64                         1.2.0-3.el7                                   docker-ce-stable
containerd.io.x86_64                         1.2.2-3.el7                                   docker-ce-stable
containerd.io.x86_64                         1.2.2-3.3.el7                                 docker-ce-stable
containerd.io.x86_64                         1.2.4-3.1.el7                                 docker-ce-stable
containerd.io.x86_64                         1.2.5-3.1.el7                                 docker-ce-stable

# GET VERSION docker-18.09.4 | docker-ce-cli-18.09.4 | containerd.io-1.2.5-3.1.el7

# ADD var version in group all/all.yml
# DOCKER
docker_version: "docker-18.09.4"
docker_ce_cli_version: "docker-ce-cli-18.09.4"
containerd_io: "containerd.io-1.2.5-3.1.el7"
```

### Install docker

cd KCDJR_ANSIBLE

ansible-playbook -i $KCDJR_ANSIBLE/kcdjr /home/ansible/project/kcdjr/ansible/roles/init_host/tasks/prerequis-os.yml

### Service Docker


``` bash
# Create space services
cd $KCDJR_ANSIBLE/roles
# create folders like below
.   
└── services
    ├── tasks
    └── templates

# create tasks services docker - retrieve file from git
cd services/tasks
vim docker_services.yaml
```
**Dont forget TAG**

ansible-playbook -i $KCDJR_ANSIBLE/kcdjr $KCDJR_ANSIBLE/roles/services/tasks/docker_services.yaml --tags [start | stop | start]

