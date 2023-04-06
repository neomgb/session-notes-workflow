# Docker 概述

## Docker 为什么出现

环境配置麻烦，不能跨平台

每一个机器都要部署环境（集群Redis、ES、Hadoop...），费时费力

项目能不能带上环境安装打包

Docker：开发打包部署上线，一套流程做完

## Docker历史

13年开源，用 Go 语言开发的

- 官网：https://www.docker.com/
- 文档：https://docs.docker.com/
- 仓库地址：https://hub.docker.com/

## Docker能干嘛

比较`Docker` 和虚拟机技术的不同：

- 传统虚拟机，虚拟出一套硬件，运行一个完整的操作系统，然后再这个系统上安装和运行软件
- 容器内的应用直接运行在宿主机内核，容器是没有自己的内核的，也没有虚拟我们的硬件，所以就轻便了
- 每个容器间是互相隔离的，每个容器内都有一个属于自己的文件系统，互不影响

# Docker 安装

## Docker的基本组成

- 镜像（image）：

  docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，通过镜像可以创建多个容器，最终服务运行或者项目运行就是在**容器**中的

- 容器（container）：
  Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建的

  启动，停止，删除

  目前可以把这个容器理解为一个简易的linux系统

- 仓库（repository）：
  仓库就是存放镜像的地方，分为私有仓库和公有仓库

  Docker Hub（默认是国外的）

  阿里云...都有容器服务器（配置镜像加速）

## 安装&卸载

**环境查看**

```
# 系统内核是 3.10 以上的
uname -r
# --------
4.15.0-106-generic
```

```shell
# 系统版本
cat /etc/os-release
# ----------
NAME="Ubuntu"
VERSION="18.04.4 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04.4 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic
```

 **安装Docker：**

1. 卸载旧版本

   ```shell
   sudo apt-get remove docker docker-engine docker.io containerd runc
   ```

2. 更新apt软件包索引

   ```
   sudo apt-get update
   ```

3. 安装apt依赖包，用于通过https来获取仓库

   ```
   sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg-agent \
        software-properties-common
   ```

4. 添加 Docker 的官方 GPG 密钥

   ```shell
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

   验证是否拥有带有指纹的密钥（可省略）

   ```shell
   sudo apt-key fingerprint 0EBFCD88
   ```

5. 设置稳定版仓库

   ```shell
   sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
   ```

6. 安装 docker-ce

   ```shell
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io
   ```

7. 设置开机自启动，安装成功后默认开启，可忽略

8. 测试运行

   ```shell
   sudo docker run hello-world
   ```

   ```shell
   # 查看一下下载的这个 hello-world 镜像
   docker images
   ```

   查看docker版本

   ```shell
   docker version
   ```

**卸载Docker**

1. 卸载依赖

   ```shell
   sudo apt-get purge docker-ce docker-ce-cli containerd.io
   ```

2. 删除资源

   ```shell
   sudo rm -rf /var/lib/docker
   ```

   > /var/lib/docker 是docker的默认工作路径

## 镜像加速

阿里云主页-左上角-产品与服务-容器镜像服务

```shell
# 这4个命令在阿里云操作页面有，直接复制过来用
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://0eunnb0b.mirror.aliyuncs.com"]
}
EOF

sudo systemctl daemon-reload

sudo systemctl restart docker
```

查看是否生效

```shell
docker info|grep Mirrors -A 1
```

# 常用命令

## 帮助命令

```shell
docker version     # 显示docker的版本信息
docker info		   # 显示docker的系统信息,包括镜像和容器的数量
docker 命令 --help  # 帮助命令
```

帮助文档的地址：https://docs.docker.com/reference/

## 镜像命令

- **docker images**  查看所有本地主机上的镜像

    ```shell
    docker images
    ```

    ```shell
    REPOSITORY       TAG       IMAGE ID         CREATED          SIZE
    hello-world      latest    bf756fb1ae65     11 months ago    13.3kB
    ```

    解释

    ```shell
    REPOSITORY  镜像的仓库源
    TAG  		镜像的标签
    IMAGE ID   	镜像的id
    CREATED  	镜像的创建时间
    SIZE  		镜像的大小
    ```
    可选项

    ```shell
    -a, --all             列出所有的镜像
    -q, --quiet           只显示镜像的id
    -aq 				  列出所有镜像的id
    ```

- **docker search**  搜索镜像

  ```shell
  docker search mysql
  ```

  ```shell
  NAME           DESCRIPTION                                     STARS       OFFICIAL    AUTOMATED
  mysql          MySQL is a widely used, open-source relation…   10202       [OK]                
  mariadb        MariaDB is a community-developed fork of MyS…   3753        [OK]                
  ```

  查看search的其他可选项

  ```
  docker search --help
  ```

  可选项

  ```shell
  --filter=STARS=3000  # 搜索出来的镜像就是STARS大于3000的
  ```

- **docker pull**  下载镜像

  帮助文档

  ```shell
  docker pull --help
  ```

  下载镜像

  ```shell
  # docker pull 镜像名[:tag]
  docker pull mysql
  ```

  ```shell
  Using default tag: latest  # 如果不写tag，默认就是latest
  latest: Pulling from library/mysql
  852e50cd189d: Pull complete # 分层下载，docker image的核心  联合文件系统
  29969ddb0ffb: Pull complete 
  a43f41a44c48: Pull complete 
  5cdd802543a3: Pull complete 
  b79b040de953: Pull complete 
  938c64119969: Pull complete 
  7689ec51a0d9: Pull complete 
  a880ba7c411f: Pull complete 
  984f656ec6ca: Pull complete 
  9f497bce458a: Pull complete 
  b9940f97694b: Pull complete 
  2f069358dc96: Pull complete 
  Digest: sha256:4bb2e81a40e9d0d59bd8e3dc2ba5e1f2197696f6de39a91e90798dd27299b093 # 签名
  Status: Downloaded newer image for mysql:latest
  docker.io/library/mysql:lates  # 真实地址
  ```

  `docker pull mysql` 等价于

  ```shell
  docker pull docker.io/library/mysql:lates
  ```

  指定版本下载

  ```shell
  docker pull mysql:5.7
  ```

  ```shell
  5.7: Pulling from library/mysql
  852e50cd189d: Already exists 
  29969ddb0ffb: Already exists 
  a43f41a44c48: Already exists 
  5cdd802543a3: Already exists 
  b79b040de953: Already exists 
  938c64119969: Already exists 
  7689ec51a0d9: Already exists 
  36bd6224d58f: Pull complete 
  cab9d3fa4c8c: Pull complete 
  1b741e1c47de: Pull complete 
  aac9d11987ac: Pull complete 
  Digest: sha256:8e2004f9fe43df06c3030090f593021a5f283d028b5ed5765cc24236c2c4d88e
  Status: Downloaded newer image for mysql:5.7
  docker.io/library/mysql:5.7
  ```

- **docker rmi** 删除镜像

  删除指定的镜像

  ```shell
  # docker rmi -f 镜像id/镜像名
  docker rmi -f ae0658fdbad5
  ```

  ```shell
  Untagged: mysql:5.7
  Untagged: mysql@sha256:8e2004f9fe43df06c3030090f593021a5f283d028b5ed5765cc24236c2c4d88e
  Deleted: sha256:ae0658fdbad5fb1c9413c998d8a573eeb5d16713463992005029c591e6400d02
  Deleted: sha256:a2cf831f4221764f4484ff0df961b54f1f949ed78220de1b24046843c55ac40f
  Deleted: sha256:0a7adcc95a91b1ec2beab283e0bfce5ccd6df590bd5a5e894954fcf27571e7f5
  Deleted: sha256:0fae465cbacf7c99aa90bc286689bc88a35d086f37fd931e03966d312d5dfb10
  Deleted: sha256:23af125b9e54a94c064bdfacc2414b1c8fba288aff48308e8117beb08b38cb19
  ```

  ```bash
  
  ```

**通过镜像名删除**

删除单个镜像

```shell
docker rmi -f jupyter/datascience-notebook:lab-2.2.9
```

删除多个镜像

  ```shell
docker rmi -f 镜像id 镜像id 镜像id
  ```

  删除所有镜像

  ```shell
docker rmi -f $(docker images -aq)
  ```

## 容器命令

> 有了镜像才可以创建容器，下载一个centos镜像来测试学习

```shell
docker pull centos
```

新建容器并启动

```shell
docker run [可选参数] image

# 参数说明
--name="Name"  容器名字，用来区分容器
-d 			   后台方式运行，类似于nohup
-it			   使用交互方式运行，进入容器查看内容
-p             指定容器的端口 -p 8080:8080
	-p ip:主机端口:容器端口
	-p 主机端口：容器端口（常用）
	-p 容器端口
	容器端口
-P			   随机指定端口
```

```shell
# 测试，启动并进入容器
docker run -it centos /bin/bash

# 从容器退回主机
exit
```

![image-20201129153127248](https://i.loli.net/2020/11/29/7DdPZtlCe6bRm3T.png)

- **docker ps**  列出所有运行的容器

  ```shell
  docker ps # 列出当前正在运行的容器
  docker ps -a  # 列出当前正在运行的容器+历史运行过的容器
  docker ps -a -n=2  # 显示最近创建的2个容器 -a参数可选
  docker ps -q  # 只显示容器的编号/id
  docker ps -aq # 显示当前所有容器的编号/id
  docker rm `docker ps -a -q`  # 删除所有的 docker ps -a 记录，删除的是镜像运行的实例
  ```

- 退出容器

  ```shell
  exit  # 退出容器并停止
  ctrl + p + q # 退出容器保持运行
  ```

- 删除容器

  ```shell
  # 删除指定的容器，不能删除正在运行的容器，强制删除用 rm -f 
  docker rm 容器id/容器name
  # 删除所有的容器
  docker rm -f $(docker ps -aq)
  # 删除所有的容器
  docker ps - a -q|xargs docker rm
  ```

- 启动和停止容器的操作

  ```shell
  docker start 容器id     # 启动容器
  docker restart 容器id   # 重启容器
  docker stop 容器id      # 停止当前正在运行的容器
  docker kill 容器id      # 强制停止当前容器
  ```

## 常用其他命令

**后台启动容器**

```shell
# 命令 docker run -d 镜像名
# 通过镜像来启动容器
docker run -d centos

# 问题 docker ps，发现 centos 停止了

# 常见的坑：docker 容器使用后台运行，就必须要有一个前台进程，
# docker发现没有应用，就会自动停止nginx，容器启动后，
# 发现自己没有提供服务，就会立刻停止，就是没有程序了
```

**查看日志**

```shell
docker logs
```

帮助文档

```shell
docker logs --help
```

```shell
docker logs -tf --tail 10 # 显示带时间戳的10行日志信息
```

**查看容器中的进程信息** 

```shell
# docker top 容器id
docker top b9ff4b6fff66
```

```shell
UID   PID     PPID      C     STIME     TTY     TIME         CMD
root  24462   24427     0     16:32     pts/0   00:00:00     /bin/bash
```

**查看镜像的元数据**

```shell
docker inspect 镜像id
```

**进入当前正在运行的容器**

```shell
# 我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

# 命令
docker exec -it 容器id
```

```shell
# 方式1 ⭐
docker exec -it b9ff4b6fff66 /bin/bash
```

```shell
# 方式2
docker attach 容器id
```

```shell
# 对比
# docker exec       # 进入容器后开启一个新的终端，可以在里面操作（常用）
# docker attach     # 进入容器正在执行的终端，不会启动新的进程
```

**从容器内拷贝文件到主机上**

```shell
docker cp 容器id:容器内路径  目的主机路径
```

```shell
# 0.运行一个docker容器
docker run -it centos /bin/bash
# 1.进入docker容器内部
docker attach 42b5790ad642
# 2.在容器内新建一个文件
cd /home
touch hello.py
# 3.使用docker cp拷贝文件到主机
docker cp 42b5790ad642:/home/hello.py /home
```

![image-20201129171823737](https://i.loli.net/2020/11/29/rtpDo214bZOeSyx.png)

> 这里的拷贝是一个手动过程，未来我们使用 `-v` 卷的技术，可以实现自动同步

## 练习1：安装Nginx

```shell
# 1.搜索镜像  search  建议去docker hub上搜索
docker search nginx
# 2.下载镜像  pull
docker pull nginx
# 3.运行测试
docker images
# -d 后台运行
# --name 给容器命名
# -p 宿主机端口：容器内部端口
docker run -d --name nginx01 -p 3344:80 nginx
docker ps
curl localhost:3344
```

> 思考：每次改动nginx配置文件，都需要进入容器内部？十分麻烦，要是可以在容器外部提供一个映射路径，达到在容器外部修改文件，容器内部就可以自动修改。 `-v` 数据卷

## 练习2：安装 Tomcat

```shell
# 1.下载镜像
docker pull tomcat
# 2.启动运行
docker run -d -p 3355:8080 --name tomcat01 tomcat
# 测试访问没有问题
# 进入容器
docker exec -it tomcat01 /bin/bash
# 发现问题：
# 1.linux命令少了 2.没有webapps
# 阿里云镜像的原因，默认是最小的镜像，所有不必要的都被剔除掉了
# 保证最小的可运行环境
# 解决方法：
# 将webspps.dist中的内容copy到webapps中
cp -r webapps.dist/* webapps
```

> 思考：以后要部署项目，如果每次都要进入容器十分麻烦，要是可以在容器外部提供一个映射路径，webapps，我们在外部放置项目，就自动同步到内部就好了！

## 练习3：部署ES+Kibana

```shell
# es 暴露的端口很多！
# es 十分的耗内存
# es 的数据一般需要防止到安全目录！挂载
#

# 启动了 linux就卡住了 docker stats 查看 cpu 的状态
```

> 思考：使用kibana连接es，网络如何才能连接过去

## 实践：安装jupyter 

```bash
# 1.下载镜像
docker pull jupyter/datascience-notebook
# 2.运行
docker run -d -p 8080:8888 jupyter/datascience-notebook
# 3.查看镜像的日志
docker logs [output above]
# 4.复制token, 输入localhost:8080
```

### 使用jupyter lab

> `jupyter/datascience-notebook` 镜像默认包括了 `jupyter lab` 扩展，可以通过以下命令运行 `jupyter lab`

```bash
docker run --name mgb-notebook -d -p 1231:8888 jupyter/datascience-notebook start.sh jupyter lab
```

[官方文档](https://jupyter-docker-stacks.readthedocs.io/en/latest/)

### 拷贝文件到容器

```bash
docker cp /root/learn dd373e9f7ef5:/home/jovyan/
```

> 查看当前目录完整路径
>
> ```
> pwd
> ```

## 可视化

- portainer（先用这个，不是最佳选择）

- Rancher（CI/CD再用）

**什么是portainer？**

Docker图像化界面管理工具，提供一个后台面板供我们操作

```shell
docker run -d -p 8088:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock  --privileged=true portainer/portainer
```

访问测试：http://ip:8088/

可视化面板平时不会使用

# Docker 镜像讲解

## 镜像是什么

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时的库、环境变量和配置文件。

所有的应用，直接打包docker镜像，就可以直接跑起来！

如何得到镜像：

- 从远程仓库下载
- 朋友拷贝
- 自己制作镜像 DockerFile

## 分层理解

Docker镜像都是**只读**的，当容器启动时，一个新的可写层被加载到镜像的顶部！

这一层就是我们通常说的容器层，容器之下的都叫镜像层！

![image-20201226110028816](https://i.loli.net/2020/12/26/f8H1CynYzBxoIcJ.png)

## commit镜像

```bash
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[TAG]
```

**实践**

```bash
# 启动jupyter镜像服务
docker run -d -p 8080:8888 jupyter/datascience-notebook
# 安装需要的包 pass
# docker ps 查看一下容器id
# 将操作后的容器通过commit提交为一个镜像，以后可以直接使用
docker commit -a="kai" -m="add pytorch" 8dc50fe86cb9 pytorch-notebook:1.0
```

> 如果想要保存当前容器的状态，就可以通过commit来提交，获得一个镜像

运行重新打包后（安装了pytorch）的镜像

```bash'
docker run --name torch-notebook -d -p 1111:8888 pytorch-notebook:1.0 start.sh jupyter lab
```

# 容器数据卷

## 什么是容器数据卷

**docker的理念**

将应用和环境打包成一个镜像！

如果数据都在容器中，那么容器被删除，数据就会丢失！==需求：数据可以持久化==

MySQL，容器删了，删库跑路！==需求：MySQL数据可以存储在本地！==

容器之间可以有一个数据共享的技术！Docker容器中产生的数据，同步到本地！

这就是**卷技术**！也就是目录的挂载，将我们容器内的目录，挂载到Linux上面！

![image-20201226114815420](https://i.loli.net/2020/12/26/ZpSwFVT7jLPhdKG.png)

 **总结一句话：容器的持久化和同步操作！==容器间也是可以数据共享的！==**

## 使用数据卷

> 方式一：直接使用命令来挂载 -v

```bash
docker run -it -v 主机目录:容器内目录
```

**实践**

> 将docker容器中的`/home` 目录挂载到服务器的 `/root/ceshi` 上，**双向绑定**

```bash
docker run -it -v /root/ceshi:/home centos /bin/bash
```

查看容器元数据

```bash
docker inspect 容器id
```

![image-20201226142631038](https://i.loli.net/2020/12/26/WHXNc8oT7bwqBRV.png)

查看文件是否同步

![image-20201226144542018](https://i.loli.net/2020/12/26/vtdoTiMOe3SCBVW.png)

停止容器，修改宿主机上的文件，重启容器查看同步效果

![image-20201226145652154](https://i.loli.net/2020/12/26/Q8ApJL2uHiMBRUd.png)

 好处：以后修改只需要在本地修改即可，容器内会自动同步！

## 实战：安装MySQL

思考：MySQL 的数据持久化问题

```bash
# 获取镜像
docker pull mysql:5.7

# 运行容器，需要做数据挂载
# 安装启动mysql 要配置密码的，需要注意
# 官方测试：
# docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
# 启动我们的
-d 后台运行
-p 端口映射
-v 挂载
-e 环境配置
--name 容器名字
docker run -d -p 2233:3306 -v /root/mysql/conf:/etc/mysql/conf.d -v /root/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7

# 启动成功之后，在本地使用sqlyog来测试一下
# 在本地测试创建一个数据库，映射的路径会自动同步
```

 假设我们将容器删除

```bash
docker rm -f mysql01
```

查看历史运行过的容器，已经找不到之前创建的`mysql01` 了

```bash
docker ps -a
```

但是挂载到本地的数据卷依然没有丢失，这就实现了容器**数据的持久化**功能

## 具名和匿名挂载

**匿名挂载**

通过 `-v 容器内路径` 

```shell
# 匿名挂载
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx

# list volumes
docker volume ls
```

**具名挂载**

通过 `-v 卷名:容器内路径` 

```shell
# 具名挂载
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
```

![image-20201226160001874](https://i.loli.net/2020/12/26/D1AbxqCNyhG9zB5.png)

查看卷位置

![image-20201226160154795](https://i.loli.net/2020/12/26/t14cVLRqjH7DrkG.png)

所有docker容器内的卷，没有指定目录的情况下都是在 `/var/lib/docker/volumes/xxxx/_data` 

![image-20201226160920680](https://i.loli.net/2020/12/26/pQfBZXczD5wGax8.png)

通过具名挂载可以方便的找到需要的卷，大多数情况都是用==具名挂载==

```bash
# 区别具名挂载、匿名挂载、指定路径挂载
-v 容器内路径  			# 匿名挂载
-v 卷名:容器内路径  	   # 具名挂载
-v /宿主机路径:容器内路径   # 指定路径挂载
```

拓展

```shell
# 通过 -v 容器内路径:ro rw 改变读写权限
ro readonly  # 只读
rw readwrite # 可读可写

# 一旦设置了容器权限，容器对我们挂载出来的内容就有限定了
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:rw nginx
```

> 只要看到 ro 就说明这个路径只能通过宿主机来操作，容器内部是无法操作的

## 初识Dockerfile

**可以通过`Dockerfile` 创建镜像，并且在通过镜像创建容器的时候挂载卷**

Dockerfile 就是用来构建 docker 镜像的构建文件！命令脚本

 通过这个脚本可以生成镜像，镜像是一层一层的，脚本则是一个个的命令，每个命令都是一层

创建一个`dockerfile1` 脚本文件，名字可以随意，建议 Dockerfile

```bash
# 指令（大写） 参数
FROM centos

VOLUME ["volume01", "volume02"]

CMD echo "---end---"
CMD /bin/bash 
# 这里的每个命令，就是镜像的一层
```

通过`dockerfile1` 脚本生成镜像

```bash
# docker build --help 查看命令的帮助文档
# -t 参数后边可以跟一个tag，不加的话默认为latest
docker build -f dockerfile1 -t kai/centos .
```

![image-20201226170926429](https://i.loli.net/2020/12/26/C5rYeRwhHOmsqL8.png)

```bash
# 从本地镜像中查看创建的镜像
docker images
```

![image-20201226171501818](https://i.loli.net/2020/12/26/hCFoisQgXWVBbnO.png)

```bash
# 运行我们创建的容器
docker run -it kai/centos /bin/bash
```

![image-20201226172612951](https://i.loli.net/2020/12/26/MXVicdOpPevaxtU.png)

这两个卷和外部一定都有一个同步的目录！且`dockerfile`脚本中挂载卷的方式为==匿名挂载==

```bash
# 查看容器的元数据
docker inspect 9b78f1b9305a
```

![image-20201226173959137](https://i.loli.net/2020/12/26/fwgMysVTBzd4UDm.png)

在容器内新建文件，找到宿主机上挂载卷的位置，查看文件是否同步出去了 

![image-20201226174659855](https://i.loli.net/2020/12/26/6DBH9dJkFnoTzgP.png)

这种方式未来用的十分多，因为我们通常会构建自己的镜像！

如果在构建镜像的时候没有挂载卷，则一般通过==具名挂载==的方式手动镜像挂载  `-v 卷名:容器内路径` 

## 容器之间使用数据卷共享文件

多个mysql 同步数据！

**通过 `--volumes-from` 挂载**

![image-20201227213413719](https://i.loli.net/2020/12/27/i4eNaEBZXKxLVjo.png)

测试：使用上面自己创建的 `kai/centos` 来启动

```bash
docker run -it --name docker01 kai/centos:latest
# 启动第2个
docker run -it --name docker02 --volumes-from docker01 kai/centos:latest
```

通过`ls -l` 的输出确认以上容器都有`volume01` 和 `volume02` 两个容器卷

```bash
# 后续测试步骤用到的操作
docker attach 容器id  # 进入容器
# ctrl + p + q 退出容器保持运行
```

在 `docker01` 中创建文件，`docker02` 中查看同步效果

在 `docker02` 中创建文件，`docker01` 中查看同步效果

发现数据是双向同步的！

启动第3个也去挂载 `docker01` 测试

```bash
docker run -it --name docker03 --volumes-from docker01 kai/centos:latest
```

`ls -l` 查看也有同样的两个数据卷

`cd volume01` ，`ls`  发现数据也是同步的！

==只要通过 `--volumes-from` 就是可以实现容器之间的数据共享了==

删除 `docker01` 发现另外两个容器中数据依然存在！

![image-20201227220750440](https://i.loli.net/2020/12/27/8jdopSHIsVmGWQB.png)

多个mysql实现数据共享

```bash
docker run -d -p 2233:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7

# 挂载mysql01中的数据卷
# 随机端口映射
docker run -d -P -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 --volumes-from mysql01 mysql:5.7

# 这个时候，可以实现两个容器数据同步
```

结论：

容器之间配置信息的传递，可以通过数据卷直接共享

数据卷容器的生命周期一直持续到没有容器使用为止

但是一旦持久化到了本地，这个时候，本地的数据是不会删除的

测试：

用不同的镜像创建容器并用 `--volumes-from` 挂载卷，发现也会同步

`volume01` 和 `volume02` 两个数据卷

# Docker 应用

## 使用jupyter lab

> `jupyter/datascience-notebook` 镜像默认包括了 `jupyter lab` 扩展，可以通过以下命令运行 `jupyter lab`

```bash
docker run --name mgb-notebook -d -p 1231:8888 jupyter/datascience-notebook start.sh jupyter lab
```

[官方文档](https://jupyter-docker-stacks.readthedocs.io/en/latest/)

**拷贝文件到容器**

```bash
docker cp /root/learn dd373e9f7ef5:/home/jovyan/
```

**查看当前目录完整路径**

```
pwd
```

