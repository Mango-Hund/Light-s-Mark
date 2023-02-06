# Docker 安装
## Docker的基本组成
**镜像（image）：**    
docker镜像就好比是一个模板，可以通过这个模板来创建服务，tomcat镜像 ==>run==>tomcat01容器（提供服务器），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中）  
**容器（container）：**       
Docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建。  
启动，停止，删除，基本命令！  
目前就可以把这个容器理解为就是一个简易的linux系统  
**仓库**  
仓库就是存放镜像的地方  
仓库分为公有仓库和私有仓库
Docker Hub 默认是国外的。

## 安装Docker
>环境准备

[官方文档](https://docs.docker.com/)   

## 使用阿里云加速
阿里云官网===>镜像服务

## Docker工作原理
Docker是一个Client-Server结构的系统，Docker的守护进程运行在主机上，通过Socket从客户访问

DockerServer接收到Docker-Client的指令，就会执行这个命令

## Docker常用命令
```shell
docker version      #显示docker版本信息
docker info         #显示docker的系统信息，包括镜像和容器的数量
```
### 镜像命令
```shell
docker images       #查看docker镜像信息

#解释
REPOSITORY 镜像的仓库源
TAG 镜像的标签
IMAGE ID  镜像的id
CREATED 镜像的创建时间
SIZE 镜像的大小

#可选项
 -a all  #列出所有镜像
 -q quiet #只显示镜像的id
```

搜索镜像
```shell
docker search mysql

可选项
--filter=STARS=3000  #选择星标大于3000的镜像
```

下载镜像
```shell
docker pull mysql

#指定镜像版本
docker pull mysql：5.7

```

删除镜像
```shell
docker rmi -f 容器id #删除指定的容器
docker rmi -f 容器1id 容器2id 容器3id #删除多个容器
docker rmi -f $(docker images -aq) #删除所有容器
```
### 容器命令
有了镜像才可以创建容器，下载一个centos来测试学习
```shell 
docker pull centos
```
新建容器并启动
```shell
# 参数说明
--name="Name"# 容器名字，用来区分容器
-d    #后台方式运行
-it   #使用交互式方式运行，进入容器查看内容
-p    #指定容器的端口 -p 8080:8080
   -p    主机端口:容器端口(常用)
   -p    容器端口
   -p    随机指定端口
   
#启动并进入容器
docker run -it centos /bin/bash
#退出
exit
ctrl + p + q 退出不停止运行
```
查看所有运行的容器
```shell
docker ps   #列出正在运行的容器
-a          #列出正在运行和历史运行的容器
-n=?        #显示最近创建的容器
-q          #只显示容器的编号
```
删除容器
```shell
docker rm 容器id
docker rm - f $(docker images) 删除所有的容器
```
启动和停止容器的操作
```shell
docker start 容器id
docker restart 重启容器
docker stop 停止当前正在运行的容器
docker kill 容器id   强制停止容器
```
### 常用的其他命令
后台启动容器
```shell
dorker run -d centos 镜像名
```
查看日志
```shell
docker logs -tf --tail 条数 容器id
# 参数解释
-tf #显示日志
--tail number 显示日志条数
```
查看进程
```shell
docker top 容器id
```
查看镜像元数据
```shell
docker inspect 容器id
```
进入当前运行的容器
```
docker exec -it 容器id /bin/bash #开启一个新终端(常用)
docker attach -it 容器id /bin/bash #进入正在执行的终端
```
从容器内拷贝文件到主机上
```shell
docker cp 容器内路径，目的主机路径
```