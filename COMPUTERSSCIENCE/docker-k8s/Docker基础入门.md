# Docker基础入门

**Docker概念**

* Docker是一个开源的应用容器引擎
* 基于`golang`语言开发
* `Docker`可以让开发者打包他们的应用以及依赖包一个轻量级、可移植的容器中，然后发布到任何流行的`Linux`机器上
* 容器是完全砂箱机制，相互隔离
* 容器性能开销极低

docker是一种容器技术，解决软件跨环境迁移的问题

**docker架构**

<img src="img/docker架构.png">

* 镜像（`Image`）：Docker镜像，就相当于是一个root文件系统
* 容器（`Container`）：镜像和容器的关系，就像是面向对象程序设计中的类和对象一样，镜像是静态的定义，容器是镜像运行时的实体
* 仓库（`Repository`）：仓库可看成一个代码控制中心，用来保存镜像

默认情况下，将从docker hub（https://hub.docker.com/）上下载docker镜像，太慢。一般都会配置镜像加速器：

* USTC：中科大镜像加速器（https://docker.mirrors.ustc.edu.cn）
* 阿里云（去阿里云官网登录可以生成自己的特有加速器）
* ...





 ## Docker命令



### docker服务相关命令

* 启动docker服务

  ```shell
  systemctl start docker
  ```

* 停止docker服务

  ```shell
  systemctl stop docker
  ```

* 重启docker服务

  ```shell
  systemctl restart docker
  ```

* 查看docker服务状态

  ```shell
  systemctl status docker
  ```

* 开机启动docker服务

  ```shell
  systemctl enable docker
  ```

  





## Docker 应用部署





## 备份与迁移





## Dockerfile





## Docker相关概念





## Docker服务编排





## Docker容器数据卷





## Docker私有仓库





