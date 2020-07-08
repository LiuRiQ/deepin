# docker安装oracle

以下镜像源亲测可用

```bash
#oracle11g版本
docker.io/arahman/docker-oracle-xe-11g
#oracle12c版本
docker.io/truevoly/oracle-12c
```

## 拉取镜像

```bash
sudo docker pull  docker.io/arahman/docker-oracle-xe-11g
```

## 查看镜像

```bash
sudo docker images
```

## 创建容器并启动

```bash
docker run -d -v /home/docker/data/oracle_data:/data/oracle_data -p 49160:22 -p 1521:1521 -e ORACLE_ALLOW_REMOTE=true docker.io/arahman/docker-oracle-xe-11g 
# -d 后台运行
# -v 将本地目录/home/docker/data/oracle_data挂载到docker的/data/oracle_data目录下
# -p 将本地端口映射到容器的虚拟端口 ：前为本地端口，：后为需要映射的虚拟端口。
# -e ORACLE_ALLOW_REMOTE表示是否允许远程连接
```

之后运行容器，可直接使用如下命令：

```bash
docker run 00a6f8bfcf00
#00a6f8bfcf00 是容器id
```

可通过

```bash
docker ps -a
```

查看所有容器的id，容器中的镜像，容器状态及容器创建时间等信息。

## 进入容器

```bash
sudo docker exec -it 00a6f8bfcf00 /bin/bash
# 00a6f8bfcf00 为容器id
```

## 连接数据库

```bash
root@00a6f8bfcf00:/# sqlplus

SQL*Plus: Release 11.2.0.2.0 Production on Tue Jul 7 17:51:45 2020

Copyright (c) 1982, 2011, Oracle.  All rights reserved.

Enter user-name: system
Enter password: 

```

**system用户，密码为oracle**

输入密码后：

```bash
Connected to:
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production

SQL> 

```

## 修改登录密码

```bash
SQL> password system
Changing password for system
Old password: 
New password: 
Retype new password: 
Password changed
```

