# Docker LAMP
![image](https://3.bp.blogspot.com/-P63_HqZZw_k/XDbYNyRevEI/AAAAAAAAFEw/zqQX4qc-aRMIua9WUKHRdrDBU4hbQnm3ACLcBGAs/s640/Docker-Lamp.jpg)

This document describe how to setup LAMP stack with Docker and Container. In this guide, We will setup three Container

- Apache and php Container

- MySQL Container

- PhpMyAdmin Container

**Step 1- Clone git repository to your local machine and change directory**

Run following command to clone reposiory to your local system.
```
git clone https://github.com/nu11secur1ty/docker-lamp
```
**Change Directroy**
```
cd docker-lamp
```

**Step 2- Setup prerequisites:**

Before, we provison our LAMP container environment. First, We need to complete some prerequisites.
1-We will setup Two Directories, One for DocumentRoot and Second for MySQL database directory.
  Run the following command to setup two directories. 
```
sh setupdir.sh
```
Copy phpinfo.php to DocumentRoot
```
cp phpinfo.php DocumentRoot
```

Once Directory setup successfully, we can proceed to provision LAMP Container stack

**Step 3- Install docker docker-compose:**
- Docker for RedHat 8
```
 curl  https://download.docker.com/linux/centos/docker-ce.repo -o /etc/yum.repos.d/docker-ce.repo
 yum makecache
 dnf makecache
 dnf update
 dnf install docker-ce
```
We need to install docker-compose if it's not already setup. follow the command below to finish the installation.
- Direct
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
- Apply executable permissions to the binary
```bash
sudo chmod +x /usr/local/bin/docker-compose
```
- Soft link
```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
- Test the installation.
```bash
docker-compose --version
```
- For OpenSuse
```bash
zypper -n in docker-compose
```

**Step 4- Setup Container LAMP stack:**

To provison LAMP stack run following command.
```
docker-compose up
```
Above command will start container in intractive mode, you can launch your container in dattach mode by this command **docker-compose up -d**

**Validate Created Container:**
```
docker ps
```
- Output
```
CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                            NAMES
326da5972785        phpmyadmin/phpmyadmin          "/run.sh supervisord…"   About an hour ago   Up About an hour    9000/tcp, 0.0.0.0:8080->80/tcp   docker-lamp_phpmyadmin_1
c1a700d81c42        nu11secur1ty/suse-apache-docker-php7   "/bin/sh -c '/usr/sb…"   About an hour ago   Up About an hour    0.0.0.0:80->80/tcp               docker-lamp_www_1
7480657c8ced        mysql:5.6                      "docker-entrypoint.s…"   About an hour ago   Up About an hour    0.0.0.0:3306->3306/tcp           docker-lamp_db_1
```
All Three Containers are up and running.
- Start containers
```bash
docker run -d nu11secur1ty/suse-apache-docker-php7
docker run -d
docker run -d
```
**Verify Apache Container and php version using phpinfophp.php file.**

In order to verify Apache container, check your Docker host IP address and access IP Address in the browser. In my case My IP address is following

http://0.0.0.0

![image](https://3.bp.blogspot.com/-zJBHaXuksvQ/XDbp8XugwrI/AAAAAAAAFE8/jOK-LTQzUl8gfSs38aynOdQF8581HSQ4gCLcBGAs/s640/ub.png)

http://0.0.0.0/phpinfo.php

![image](https://4.bp.blogspot.com/-1CzCJtqwCd8/XDbqNfCXhFI/AAAAAAAAFFE/o1PIqBZbBtcRnZ1mh2srVC5T4rYVZsQwACLcBGAs/s640/ubb.png)

**Verify PhpMyAdmin access**

To check PhpMyAdmin login, browse Docker host IP with port 8080

http://0.0.0.0:8080

Enter the User **root** and Password is **test** that we defined in our **docker-compose.yaml** file.

After successful authentication, your output would be like below screenshot

![image](https://1.bp.blogspot.com/-XamxqDQWAGY/XDbrncOgA2I/AAAAAAAAFFQ/ZfYhmfyD_qY_wEebQghH1Zq3ENi5LSAowCLcBGAs/s640/ubbb.png)

------------------------------------------------------------------------------------------------------------------------

**docker save**

- Create a backup that can then be used with docker load

```bash
$ docker save busybox > busybox.tar

$ ls -sh busybox.tar

2.7M busybox.tar

$ docker save --output busybox.tar busybox

$ ls -sh busybox.tar

2.7M busybox.tar

$ docker save -o fedora-all.tar fedora

$ docker save -o fedora-latest.tar fedora:latest
```

---------------------------------------------------------------------------


**docker load**

- Examples

```bash
$ docker image ls

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE

$ docker load < busybox.tar.gz

Loaded image: busybox:latest
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
busybox             latest              769b9341d937        7 weeks ago         2.489 MB

$ docker load --input fedora.tar

Loaded image: fedora:rawhide

Loaded image: fedora:20

$ docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
busybox             latest              769b9341d937        7 weeks ago         2.489 MB
fedora              rawhide             0d20aec6529d        7 weeks ago         387 MB
fedora              20                  58394af37342        7 weeks ago         385.5 MB
fedora              heisenbug           58394af37342        7 weeks ago         385.5 MB
fedora              latest              58394af37342        7 weeks ago         385.5 MB
```

