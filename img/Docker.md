# Docker的安装与配置

## 购买服务器

https://home.console.aliyun.com/home/dashboard/ResourceDashboard

创建安全组（类似于防火墙）用来开启端口，否则外部无法访问。端口映射

获取服务器公网IP地址，修改实例名称和密码（离线重置），重启。

## 使用xshell远程连接。

![image-20230429015816919](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230429015816919.png)

连接成功

连接之后就是Linux标准操作

## 安装docker

### [文档](https://docs.docker.com/engine/install/centos/)

跟着文档用命令安装即可

在xshell上粘贴是shift+insert

查看版本

![image-20230429033508136](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230429033508136.png)



检查卸载旧版本

![image-20230429033718122](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230429033718122.png)



安装包

```
yum -y install 包名
```



镜像网站

![image-20230429033834249](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230429033834249.png)



其余步骤跟着文档即可



## 启动docer

```
systemctl start docker
```



查看版本

```
docker version
```



## 使用docker运行第一个程序

```
docker run hello-world
```



查看镜像

```
docker image
```

![image-20230429035302531](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230429035302531.png)



## 阿里云镜像加速

容器镜像服务

![image-20230501103008648](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230501103008648.png)

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://abvgosb3.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



## ubuntu安装docker

```shell
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common lrzsz -y
```

阿里云的源

```shell
sudo curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```

软件源升级

```shell
sudo apt-get update
```

安装docker

```shell
sudo apt-get install docker-ce -y
```

测试docker

```shell
docker container run hello-world 
```



![](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230530195057331.png)





# Docker概述

隔离机制

基于GO语言开发的开源项目

仓库地址：[Docker Hub](https://hub.docker.com/)

虚拟机技术

共用

![image-20230508201829418](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230508201829418.png)

Docker

容器化技术

隔离

![image-20230508202029373](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230508202029373.png)



# Docker基本组成

![image-20230508203005442](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230508203005442.png)

镜像（image）：一个模板，通过模板来创建（多个）容器服务

容器（container）：利用容器技术，独立运行一个或者一个组应用，通过镜像来创建。简易的linux系统。

仓库（repository）：存放镜像的地方，公有仓库（Docker Hub,阿里云 .....），私有仓库





# run的流程

![image-20230612160123521](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230612160123521.png)



# 底层原理

![image-20230612160554834](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230612160554834.png)





# docker基本命令

## 帮助命令

[帮助文档](https://docs.docker.com/reference/)

```shell
docker version	#显示docker版本信息
docker info		#显示docker的系统信息，包括镜像和容器的数量
docker 命令 --help	#帮助命令
docker --help  
```



## 镜像命令

[docker images](https://docs.docker.com/engine/reference/commandline/images/)



 ```shell 
root@hostname:~# docker images --help

Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Aliases:
  docker image ls, docker image list, docker images

# 可选项
Options:
  -a, --all             #列出所有的镜像
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Format output using a custom template:
                        'table':            Print output in table format with column headers (default)
                        'table TEMPLATE':   Print output in table format using the given Go template
                        'json':             Print in JSON format
                        'TEMPLATE':         Print output using the given Go template.
                        Refer to https://docs.docker.com/go/formatting/ for more information about
                        formatting output with templates
      --no-trunc        Don't truncate output
  -q, --quiet           #只显示镜像的ID

 ```

```shell
root@hostname:~# docker images -a
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    9c7a54a9a43c   5 weeks ago   13.3kB
root@hostname:~# docker images -q
9c7a54a9a43c
root@hostname:~# docker images -aq	#显示所有的镜像
9c7a54a9a43c
```





### docker images 查看本地主机上的镜像

```shell
root@hostname:~# docker images
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    9c7a54a9a43c   5 weeks ago   13.3kB

#
REPOSITORY    TAG         IMAGE ID     CREATED       SIZE
镜像的仓库源	  镜像的标签    镜像的ID	  镜像的创建时间  镜像的大小
```



### docker search 搜索镜像

[网页搜索](https://hub.docker.com/_/mysql)

```shell 
root@hostname:~# docker search mysql
NAME                            DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                           MySQL is a widely used, open-source relation…   14221     [OK]       
mariadb                         MariaDB Server is a high performing open sou…   5431      [OK]       
percona                         Percona Server is a fork of the MySQL relati…   614       [OK]       
phpmyadmin                      phpMyAdmin - A web interface for MySQL and M…   818       [OK]       
bitnami/mysql                   Bitnami MySQL Docker Image                      89                   [OK]
circleci/mysql                  MySQL is a widely used, open-source relation…   29                   
bitnami/mysqld-exporter                                                         5                    
ubuntu/mysql                    MySQL open source fast, stable, multi-thread…   49                   
cimg/mysql                                                                      0                    
rapidfort/mysql                 RapidFort optimized, hardened image for MySQL   23                   
rapidfort/mysql8-ib             RapidFort optimized, hardened image for MySQ…   9                    
google/mysql                    MySQL server for Google Compute Engine          23                   [OK]
hashicorp/mysql-portworx-demo                                                   0                    
rapidfort/mysql-official        RapidFort optimized, hardened image for MySQ…   7                    
newrelic/mysql-plugin           New Relic Plugin for monitoring MySQL databa…   1                    [OK]
databack/mysql-backup           Back up mysql databases to... anywhere!         86                   
linuxserver/mysql               A Mysql container, brought to you by LinuxSe…   38                   
bitnamicharts/mysql                                                             0                    
mirantis/mysql                                                                  0                    
docksal/mysql                   MySQL service images for Docksal - https://d…   0                    
linuxserver/mysql-workbench                                                     50                   
vitess/mysqlctld                vitess/mysqlctld                                1                    [OK]
eclipse/mysql                   Mysql 5.7, curl, rsync                          0                    [OK]
drupalci/mysql-5.5              https://www.drupal.org/project/drupalci         3                    [OK]
drupalci/mysql-5.7              https://www.drupal.org/project/drupalci         0                    
```





```shell
root@hostname:~# docker search --help

Usage:  docker search [OPTIONS] TERM

Search Docker Hub for images

Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results
      --no-trunc        Don't truncate output

#可选项，通过收藏数来过滤
docker search mysql --filter=STARS=3000
root@hostname:~# docker search mysql --filter=STARS=3000	#搜索出来的镜像是收藏大于等于3000的
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   14221     [OK]       
mariadb   MariaDB Server is a high performing open sou…   5431      [OK]   
```





### docker pull 下载镜像

```shell
root@hostname:~# docker pull --help

Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]

Download an image from a registry

Aliases:
  docker image pull, docker pull

Options:
  -a, --all-tags                Download all tagged images in the repository
      --disable-content-trust   Skip image verification (default true)
      --platform string         Set platform if server is multi-platform capable
  -q, --quiet                   Suppress verbose output
```



下载镜像 `docker pull 镜像名[:tag]`

```shell
root@hostname:~# docker pull mysql
Using default tag: latest		#如果不写tag，默认是latest
latest: Pulling from library/mysql
72a69066d2fe: Pull complete 	#分层下载，docker image 的核心 联合文件系统
93619dbc5b36: Pull complete 
99da31dd6142: Pull complete 
626033c43d70: Pull complete 
37d5d7efb64e: Pull complete 
ac563158d721: Pull complete 
d2ba16033dad: Pull complete 
688ba7d5c01a: Pull complete 
00e060b6d11d: Pull complete 
1c04857f594f: Pull complete 
4d7cfa90e6ea: Pull complete 
e0431212d27d: Pull complete 
Digest: sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709	#签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest	#真实地址
```

```shell
#二者等价
docker pull mysql
docker pull docker.io/library/mysql:latest
```

```shell
#指定版本下载
root@hostname:~# docker pull mysql:5.7
5.7: Pulling from library/mysql
72a69066d2fe: Already exists 	#这里就体现了分层的优势，可以共用，节省内存
93619dbc5b36: Already exists 
99da31dd6142: Already exists 
626033c43d70: Already exists 
37d5d7efb64e: Already exists 
ac563158d721: Already exists 
d2ba16033dad: Already exists 
0ceb82207cd7: Pull complete 
37f2405cae96: Pull complete 
e2482e017e53: Pull complete 
70deed891d42: Pull complete 
Digest: sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

![image-20230612171248122](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230612171248122.png)



### docker rmi 删除镜像

删除指定容器	`docker rmi -f  容器id`

删除多个指定容器	`docker rmi -f 容器id 容器id 容器id`

```shell
root@hostname:~# docker rmi -f 3218b38490ce
Untagged: mysql:latest
Untagged: mysql@sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709
Deleted: sha256:3218b38490cec8d31976a40b92e09d61377359eab878db49f025e5d464367f3b
Deleted: sha256:aa81ca46575069829fe1b3c654d9e8feb43b4373932159fe2cad1ac13524a2f5
Deleted: sha256:0558823b9fbe967ea6d7174999be3cc9250b3423036370dc1a6888168cbd224d
Deleted: sha256:a46013db1d31231a0e1bac7eeda5ad4786dea0b1773927b45f92ea352a6d7ff9
Deleted: sha256:af161a47bb22852e9e3caf39f1dcd590b64bb8fae54315f9c2e7dc35b025e4e3
Deleted: sha256:feff1495e6982a7e91edc59b96ea74fd80e03674d92c7ec8a502b417268822ff
```

删除全部容器 `docker rmi -f $(docker images -aq)`

```shell
root@hostname:~# docker rmi -f $(docker images -aq)
Untagged: hello-world:latest
Untagged: hello-world@sha256:fc6cf906cbfa013e80938cdf0bb199fbdbb86d6e3e013783e5a766f50f5dbce0
Deleted: sha256:9c7a54a9a43cca047013b82af109fe963fde787f63f9e016fdc3384500c2823d
Untagged: mysql:5.7
Untagged: mysql@sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Deleted: sha256:c20987f18b130f9d144c9828df630417e2a9523148930dc3963e9d0dab302a76
Deleted: sha256:6567396b065ee734fb2dbb80c8923324a778426dfd01969f091f1ab2d52c7989
Deleted: sha256:0910f12649d514b471f1583a16f672ab67e3d29d9833a15dc2df50dd5536e40f
Deleted: sha256:6682af2fb40555c448b84711c7302d0f86fc716bbe9c7dc7dbd739ef9d757150
Deleted: sha256:5c062c3ac20f576d24454e74781511a5f96739f289edaadf2de934d06e910b92
Deleted: sha256:8805862fcb6ef9deb32d4218e9e6377f35fb351a8be7abafdf1da358b2b287ba
Deleted: sha256:872d2f24c4c64a6795e86958fde075a273c35c82815f0a5025cce41edfef50c7
Deleted: sha256:6fdb3143b79e1be7181d32748dd9d4a845056dfe16ee4c827410e0edef5ad3da
Deleted: sha256:b0527c827c82a8f8f37f706fcb86c420819bb7d707a8de7b664b9ca491c96838
Deleted: sha256:75147f61f29796d6528486d8b1f9fb5d122709ea35620f8ffcea0e0ad2ab0cd0
Deleted: sha256:2938c71ddf01643685879bf182b626f0a53b1356138ef73c40496182e84548aa
Deleted: sha256:ad6b69b549193f81b039a1d478bc896f6e460c77c1849a4374ab95f9a3d2cea2
```



## 容器命令

有了镜像才可以创建容器

下载centos镜像

```shell
docker pull centos
```



新建容器并启动

```shell
docker run [可选参数] images
```

参数说明

```shell
--name="Name"	容器名字 tomcat01 tomcat02, 用来区分容器
-d				后台方式交互
-it				使用交互方式运行，进入容器查看内容
-p				指定容器的端口 -p 8080:8080
	-p ip:主机端口:容器端口
	-p 主机端口:容器端口  （常用）
	-p 容器端口
	容器端口
-p				随机指定端口
```

启动并进入容器

```shell
root@hostname:~# docker run -it centos /bin/bash
[root@2a2e62837d69 /]# ls	#查看容器内的centos，基础版本，很多命令都是不完善的
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

列出所有运行的容器

`docker ps 可选命令`

```shell
docker ps	#列出当前正在运行的容器
-a			#列出当前正在运行的容器+带出历史运行过的容器
-n=?		#显示最近创建的容器
-q			#只显示容器的编号

root@hostname:~# docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS                     PORTS     NAMES
2a2e62837d69   centos         "/bin/bash"   13 minutes ago   Exited (0) 7 minutes ago             unruffled_tharp
f38d03325ebc   9c7a54a9a43c   "/hello"      2 hours ago      Exited (0) 2 hours ago               naughty_hypatia
3213a61d778e   9c7a54a9a43c   "/hello"      12 days ago      Exited (0) 12 days ago               optimistic_meninsky
aace9846c6e0   9c7a54a9a43c   "/hello"      12 days ago      Exited (0) 12 days ago               happy_liskov
```

退出容器

```shell
exit	#直接容器停止并退出
Ctrl+P+Q	#容器不停止退出
```

删除容器

```shell
docker rm 容器id					  #删除指定的容器，不能删除正在运行的容器，如果要强制删除，rm -f
docker rm -f $(docker ps -aq)		#删除所有的容器
docker ps -a -q|xargs docker rm		#删除所有的容器
```

启动和停止容器

```shell
docker start 容器id		#启动容器
docker restart 容器id		#重启
docker stop 容器id		#停止当前正在运行的容器
docker kill 容器id		#强制停止当前容器
```

## 常用其他命令

后台启动容器

```shell
#命令 docker run -d 镜像名
root@hostname:~# docker run -d centos

#问题：docker ps ，发现centos停止了

#常见的坑：docker 容器使用后台运行，就必须要有一个前台进程，docker发现没有应用，就会自动停止
#nginx，容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了。
```

查看日志

```shell
docker logs -f -t --tail 10 容器
```

```shell
#自己写一段shell脚本
docker run -d centos /bin/sh -c "while true;do echo hahaha;sleep 1;done"

root@hostname:~# docker ps
CONTAINER ID   IMAGE     COMMAND                
bd3be72a6323   centos    "/bin/sh -c 'while t…"   
fcb111002cd6   centos    "/bin/bash"                   

#显示日志
-tf	#显示日志
--tail number	#显示日志条数
root@hostname:~# docker logs -tf --tail 10 bd3be72a6323 
2023-06-27T09:38:27.541926586Z hahaha
2023-06-27T09:38:28.544235003Z hahaha
2023-06-27T09:38:29.546342091Z hahaha
2023-06-27T09:38:30.548168308Z hahaha
......
```

查看容器中的进程信息

```shell
#命令docekr top 容器id
root@hostname:~# docker top bd3be72a6323
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                89532               89510               0                   17:37               ?                   00:00:00            /bin/sh -c while true;do echo hahaha;sleep 1;done
root                90144               89532               0                   17:46               ?                   00:00:00            /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
```

查看镜像的元数据

```shell
docker inspect 容器id
root@hostname:~# docker inspect bd3be72a6323
[
    {
        "Id": "bd3be72a632307cb6e2d146b71ebeb339da2633f19edebca03f6cebcd5546825",
        "Created": "2023-06-27T09:37:00.084317911Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo hahaha;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 89532,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2023-06-27T09:37:00.340904059Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "ResolvConfPath": "/var/lib/docker/containers/bd3be72a632307cb6e2d146b71ebeb339da2633f19edebca03f6cebcd5546825/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/bd3be72a632307cb6e2d146b71ebeb339da2633f19edebca03f6cebcd5546825/hostname",
        "HostsPath": "/var/lib/docker/containers/bd3be72a632307cb6e2d146b71ebeb339da2633f19edebca03f6cebcd5546825/hosts",
        "LogPath": "/var/lib/docker/containers/bd3be72a632307cb6e2d146b71ebeb339da2633f19edebca03f6cebcd5546825/bd3be72a632307cb6e2d146b71ebeb339da2633f19edebca03f6cebcd5546825-json.log",
        "Name": "/wizardly_euler",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                30,
                138
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/ce5e607b1d0b3f6ed13657820373cbd60c5b8ab899e9960009518720ea842cb1-init/diff:/var/lib/docker/overlay2/5c9d77e55ba5d0cc9aa9d155dba712566ee84d9568b9014a97d2fedbc4d5f7cb/diff",
                "MergedDir": "/var/lib/docker/overlay2/ce5e607b1d0b3f6ed13657820373cbd60c5b8ab899e9960009518720ea842cb1/merged",
                "UpperDir": "/var/lib/docker/overlay2/ce5e607b1d0b3f6ed13657820373cbd60c5b8ab899e9960009518720ea842cb1/diff",
                "WorkDir": "/var/lib/docker/overlay2/ce5e607b1d0b3f6ed13657820373cbd60c5b8ab899e9960009518720ea842cb1/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "bd3be72a6323",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true;do echo hahaha;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20210915",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "27cabbe7d1a50ce0f98d6218f7c681497ec6cbafb3b4145b54022575c12ec671",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/27cabbe7d1a5",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "3522ada235352ba0801e70af4e302acd0166b541eb6cdf714c1d3ae6ce6dd7d4",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.3",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:03",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "d44465cf9a1845d21937c16ee9802bedbfad36a1a340a655473413269e7c4b56",
                    "EndpointID": "3522ada235352ba0801e70af4e302acd0166b541eb6cdf714c1d3ae6ce6dd7d4",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:03",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

## 进入当前正在运行的容器

```shell
#我们通常容器都是后台方式运行，需要进入容器，修改一些配置

#命令
docker exec -it 容器id bashShell

#测试
root@hostname:~# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
bd3be72a6323   centos    "/bin/sh -c 'while t…"   19 minutes ago   Up 19 minutes             wizardly_euler
fcb111002cd6   centos    "/bin/bash"              26 minutes ago   Up 26 minutes             happy_lewin
root@hostname:~# docker exec -it bd3be72a6323 /bin/bash
[root@bd3be72a6323 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@bd3be72a6323 /]# ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 09:37 ?        00:00:00 /bin/sh -c while true;do echo hahaha;sleep 1;done
root        1252       0  0 09:57 pts/0    00:00:00 /bin/bash
root        1286       1  0 09:58 ?        00:00:00 /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
root        1287    1252  0 09:58 pts/0    00:00:00 ps -ef

#方式二
docker attach 容器id
#测试
root@hostname:~# docker attach bd3be72a6323  
正在执行当前的代码...

#区别
#docker exec		#进入容器后开启一个新的终端，可以在里面操作（常用）
#docker attach 		#进入容器正在执行的终端，不会启动新的进程
```

从**容器内**拷贝文件到**主机上**

```shell
root@hostname:~# docker run -it centos /bin/bash
[root@e2f7039e7c3a /]# root@hostname:~# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS              PORTS     NAMES
e2f7039e7c3a   centos    "/bin/bash"   About a minute ago   Up About a minute             angry_curie
root@hostname:~# cd /home
root@hostname:/home# ls
root@hostname:/home# touch haha.java
root@hostname:/home# ls
haha.java
root@hostname:/home# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
e2f7039e7c3a   centos    "/bin/bash"   3 minutes ago   Up 3 minutes             angry_curie
#进入docker容器内部
root@hostname:/home# docker attach e2f7039e7c3a  
[root@e2f7039e7c3a /]# cd /home
[root@e2f7039e7c3a home]# ls
#在容器内新建一个文件
[root@e2f7039e7c3a home]# touch test.java
[root@e2f7039e7c3a home]# exit
exit
root@hostname:/home# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
root@hostname:/home# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS                      PORTS     NAMES
e2f7039e7c3a   centos    "/bin/bash"   5 minutes ago   Exited (0) 15 seconds ago             angry_curie
#将这个文件拷贝出来到主机上
root@hostname:/home# docker cp e2f7039e7c3a:/home/test.java /home
Successfully copied 1.54kB to /home
root@hostname:/home# ls
haha.java  test.java

#拷贝是一个手动过程，未来我们使用 -v 卷的技术，可以实现
```

## 小结

![image-20230627225516197](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230627225516197.png)

![image-20230627225657510](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230627225657510.png)

![image-20230627225733892](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230627225733892.png)


