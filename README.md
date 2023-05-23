# deployment-docker-compose

Deploying stacks using Docker Compose.

The environment for setting up is as follows:

```bash
$ sudo dmidecode -t 1

# dmidecode 3.2
Getting SMBIOS data from sysfs.
SMBIOS 2.7 present.

Handle 0x0001, DMI type 1, 27 bytes
System Information
        Manufacturer: VMware, Inc.
        Product Name: VMware Virtual Platform
        Version: None
        Serial Number: VMware-56 4d 47 03 53 98 53 75-69 d8 3e bf e6 82 89 a7
        UUID: 03474d56-9853-7553-69d8-3ebfe68289a7
        Wake-up Type: Power Switch
        SKU Number: Not Specified
        Family: Not Specified
```

```bash
$ cat /etc/lsb-release

DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04.4 LTS"
```

```bash
$ sudo dmidecode |grep -A16 "System Information$"

System Information
        Manufacturer: VMware, Inc.
        Product Name: VMware Virtual Platform
        Version: None
        Serial Number: VMware-56 4d 47 03 53 98 53 75-69 d8 3e bf e6 82 89 a7
        UUID: 03474d56-9853-7553-69d8-3ebfe68289a7
        Wake-up Type: Power Switch
        SKU Number: Not Specified
        Family: Not Specified

Handle 0x0002, DMI type 2, 15 bytes
Base Board Information
        Manufacturer: Intel Corporation
        Product Name: 440BX Desktop Reference Platform
        Version: None
        Serial Number: None
        Asset Tag: Not Specified
```

```bash
$ lsblk

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop0    7:0    0     4K  1 loop /snap/bare/5
loop1    7:1    0 302.2M  1 loop /snap/code/129
loop2    7:2    0 349.7M  1 loop /snap/gnome-3-38-2004/137
loop3    7:3    0 349.7M  1 loop /snap/gnome-3-38-2004/140
loop4    7:4    0  73.1M  1 loop /snap/core22/634
loop5    7:5    0    46M  1 loop /snap/snap-store/638
loop6    7:6    0  65.2M  1 loop /snap/gtk-common-themes/1519
loop7    7:7    0  91.7M  1 loop /snap/gtk-common-themes/1535
loop9    7:9    0  53.2M  1 loop /snap/snapd/19122
loop10   7:10   0  49.9M  1 loop /snap/snapd/18596
loop11   7:11   0  63.3M  1 loop /snap/core20/1852
loop12   7:12   0  63.5M  1 loop /snap/core20/1891
loop13   7:13   0  12.3M  1 loop /snap/snap-store/959
loop14   7:14   0 460.6M  1 loop /snap/gnome-42-2204/102
sda      8:0    0    40G  0 disk 
├─sda1   8:1    0   512M  0 part /boot/efi
├─sda2   8:2    0     1K  0 part 
└─sda5   8:5    0  39.5G  0 part /
sr0     11:0    1  1024M  0 rom 
```

## Setting up the environment.

```bash
$ sudo apt install net-tools
# check ip information
``` 

install ssh

```bash
$ sudo apt-get install openssh-client
$ sudo apt-get install openssh-server
$ /etc/init.d/ssh start
```

config ssh
```bash
$ sudo vim /etc/ssh/sshd_config
# PermitRootLogin yes

$ service ssh restart

# set password
$ sudo passwd root
```

install docker

```bash
$ sudo apt-get update
$ sudo apt-get remove docker docker-engine docker.io containerd runc
$ sudo apt-get purge docker-ce docker-ce-cli containerd.io
$ sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
$ echo   "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

$ sudo apt-get install docker-ce docker-ce-cli containerd.io
$ sudo gpasswd -a kiga docker
$ sudo reboot
```

test container

```bash
curl -H "Content-Type:application/json" -X POST -d '{"username": "admin", "password": "password"}' http://192.168.123.128:32768/api/iam/v1/auth/login
```

install docker-compose

```bash
$ sudo curl -L https://raw.githubusercontent.com/docker/compose/3.8/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
$ sudo apt-get install docker-compose-plugin
```

## Deployment dev environmnet

By default, the ${ENV} variable is set to dev.

If you need to specify a different environment, you can add the ENV=xxx parameter, where xxx is the name of the environment you want to set. For example, if you want to set the environment to prod, you can use the following command:

```bash
sudo make deploy ENV=prod
```

## Run deployment

```bash
sudo make run ENV=dev
```

If deploying to multiple environments, it's necessary to reset the previous environment by either using the "make clean" command or manually removing the mounted directory before deploying to the next environment again.


TODO 

Trying to modify the IAM service and reset the username and password of MySQL

## clean docker-compose

```bash
$ make clean
```
