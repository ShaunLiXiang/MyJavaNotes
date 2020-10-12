Docker虚拟机

# Step One

登录 用户名 root 密码123456

查看虚拟机ip地址 IP addr  172.17.0.1

Docker的版本必须高于3.10  命令 

```shell
uname -r
```

升级软件包及内核

```shell
yum update
```

安装docker

```shell
yum install docker
```

启动docker

```shell
systemctl start docker
```

将docker服务设为开机启动

```shell
systemctl enable docker
```

docker 守护线程重启

```powe
sudo systemctl daemon-reload
```

#### systemctl 方式

守护进程重启

```powershell
sudo systemctl daemon-reload
```

 重启docker服务

 ```powershell
 sudo systemctl restart docker
 ```

 关闭docker服务

```powershell
 sudo systemctl stop docker
```

#### service 方式

重启docker服务

```powershell
sudo service docker restart sudo service docker stop
```


 关闭docker服务

```powershell
sudo service docker stop
```





# Step Two

## 镜像

查询镜像

```shell
docker search redis/mysql
```

拉取镜像

```shell
docker pull redis/mysql 默认latest
个人甘炯 8.0.35这个版本挺好的
docker pull mysql:5.5 	拉取5.5
```

查询镜像

```shell
docker images
```

删除镜像

docker rmi image-id

## 容器

容器启动

```shell
docker run --name container-name(容器名) -d(后台运行) image-name(镜像名)
eg:docker run --name myredis -d -redis
```

查询运行中的容器

```shell
docker ps
所有
docker ps -a
```

停止运行中的容器

```shell
docker stop container-name/container-id
```

启动容器

```shell
docker start container-name/container-id
```

删除容器

```shell
docker rm container-id
```

端口映射

```shell
-p 6379:6379
将虚拟机的端口映射到容器的端口
eg: docker run -d -p 6379:6379 --name myredis docker.io/redis

```

容器日志

```shell
docker logs container-name/container-id
```

虚拟机防火墙状态

```shell
service firewall status
```

关闭虚拟机防火墙

```shell
service firewall stop
```

更多命令查看

https:docs.docker.com/engine/reference/commandline/docker/

# 一、DOCKER安装

**1. 查看CentOs版本**

```powershell
uname -r
# Dcoker要求CentOs系统的内核版本高于3.10
12
```

**2. 升级内核(非必须)**

```powershell
yum update
```

**3. 安装Docker**

```powershell
# 默认会下载最新版的Docker
yum install docker
12
```

**4. 启动Docker**

```powershell
yum install docker
1
```

**如果启动过程中没有报错，说明Docker安装启动完成，可以用`docker -v`查看docker版本。
如果启动过程中出现如下错误：**

```powershell
Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service" and "journalctl -xe" for details.
1
```

**可以按照提示，输入`systemctl status docker.service`命令查看错误提示。**
![在这里插入图片描述](https://www.freesion.com/images/807/330c796de09f26443d14a39f7ed433b7.png)**如果是以上错误，说明SELinux不支持此内核上的overlay2图形驱动程序，将selinux禁用即可。
禁用步骤：**
**1、输入以下命令**

```powershell
vi /etc/sysconfig/docker
```

**2、将selinux-enabled=false**
![在这里插入图片描述](https://www.freesion.com/images/434/897fcada659af4102c7c25735e511cf2.png)

**然后再次启动Dcoker即可，如果还是无法启动，可以考虑升级CentOs内核版本。**

