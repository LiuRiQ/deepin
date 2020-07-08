# docker卸载与安装

## 卸载

### 1.卸载docker-ce

```bash
sudo apt-get remove docker docker-ce
```

### 2.查看docker的文件位置

```bash
whereis docker
```

结果如下：

```bash
docker: /usr/bin/docker /etc/docker /usr/libexec/docker /usr/share/man/man1/docker.1.gz
```

### 3.删除docker文件

使用`rm -rf`命令删除这些文件

如：

```bash
sudo rm -rf /usr/bin/docker
```

删除后，可以使用`docker`或`which`命令

```bash
dream@dream-PC:~$ docker
bash: docker: 未找到命令
dream@dream-PC:~$ which docker
dream@dream-PC:~$ 
```

可以看到docker已被卸载

## 安装

### 1.安装docker密钥

```bash
dream@dream-PC:~$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
OK
```

### 2.查看秘钥是否安装成功

```bash
dream@dream-PC:~$ sudo apt-key fingerprint 0EBFCD88
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

### 3.添加docker官方仓库

```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian wheezy stable"
```

这里会报错，但不会影响docker正常安装

```bash
aptsources.distro.NoDistroTemplateException: Error: could not find a distribution template for Deepin/camel
```

### 4.安装`docker-ce`

```bash
sudo apt-get install docker-ce
```

### 5.查看docker版本

```bash
dream@dream-PC:~$ docker version
Client: Docker Engine - Community
 Version:           19.03.12
 API version:       1.40
 Go version:        go1.13.10
 Git commit:        48a66213fe
 Built:             Mon Jun 22 15:45:52 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.12
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.10
  Git commit:       48a66213fe
  Built:            Mon Jun 22 15:44:23 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

### 6.运行hello-world容器

```bash
dream@dream-PC:~$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:d58e752213a51785838f9eed2b7a498ffa1cb3aa7f946dda11af39286c3db9a9
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

可以看到15行，显示docker可以正常运行了。

由于系统盘容量较小，所以想将docker的相关文件移动到数据盘。

## 修改docker数据目录

修改/etc/docker/daemon.json

若没有daemon.json文件，使用以下命令可自动创建

```bash
dream@dream-PC:~$ sudo vim /etc/docker/daemon.json
```

在该文件中写入以下数据

```json
{
    "data-root": "/home/dream/docker"
}
```

目录可以自定义

重启docker

```bash
systemctl restart docker
```

查看该目录，可以看到相关文件

```bash
dream@dream-PC:~$ cd /home/dream/docker/
dream@dream-PC:~/docker$ ls
builder   containers  network   plugins   swarm  trust
buildkit  image       overlay2  runtimes  tmp    volumes
dream@dream-PC:~/docker$ 
```

## 添加docker镜像源

还是修改`daemon.json`文件

在文件中加入镜像源地址，`daemon.json`内容如下：

```json
{
    "data-root": "/home/dream/docker",
    "registry-mirrors": ["http://registry.docker-cn.com",
        "http://hub-mirror.c.163.com",
        "http://docker.mirrors.ustc.edu.cn",
        "http://docker.mirrors.ustc.edu.cn"]
}
```

