---
title: 快速开始
weight: 2
---

{{< toc >}}

## 1. 说明

本文将从零开始，让您快速的部署**颠勺**并构建第一个 Yocto 项目。

本文主要目的是让您快速体验**颠勺**，希望在短时间内，可以说服您这个项目对嵌入式 Linux 项目开发是非常有用，可以提供生产力的。

详细的使用说明请参考颠勺用户手册，里面会更加详细的介绍颠勺所有的功能以及您最好掌握的相关知识说明。

## 2. 安装

### 2.1 资源要求

由于需要编译 Yocto 项目，所以对资源要求较高，具体要求如下

- 内存最小 4GB，推荐使用 8GB 及以上
- 硬盘剩余空间大于 80GB

同时，为了提高编译效率，需要较多的 CPU 核，根据我个人的使用经验，10核以下机器编译体验较差。因此不建议长期使用个人电脑运行颠勺，编译效率过低往往会打消您的热情，影响您的工作效率。推荐将颠勺运行于服务器或者工作站之上，一台普通的16核32G的服务器即可满足要求。

除此之外，非常重要的一点是请确保您的网络可以通畅连接到各个源码仓库。Yocto 项目采用完全从源码编译的方式构建项目，因此第一次编译时需要拉取项目包含的软件源码包。如果您的机器无法很好的访问这些源码仓库，你会遇到多次失败，或者等待一周甚至更长时间也无法完成编译。

工欲善其事必先利其器，建议您将宿主机的资源和网络准备好后再开始使用颠勺

### 2.2 安装准备

目前，颠勺已经在 Windows (Win10 & Win11) 和 Linux (Ubuntu, Fedora, CentOS 8) 环境下完成测试。你可以选择你习惯的操作系统环境作为 Docker 主机， 请根据如下官方文档安装 Docker & Docker-Compose

[docker 安装说明](https://docs.docker.com/engine/install/)

[docker-compose 安装说明](https://docs.docker.com/compose/install/)

### 2.3 安装

1. 下载项目
    
    *在 Linux 环境中使用*
   ```sh
   $ git clone https://github.com/croakexciting/dianshao.git && cd ./dianshao
   ```

   *注意：由于 Bitbake 无法被 root 用户使用，因此请确保项目文件夹权限为 1000：1000*

    *在 Windows 环境中使用*

    ```sh
    $ git clone https://github.com/croakexciting/dianshao.git -c core.autocrlf=false
    
    $ cd ./dianshao
    ```

2. 设置你的 Yocto Project 项目存放路径
   ```sh
   $ export DIANSHAO_YOCTO_PROJECT_PATH="your yocto project path"
   ```
   *注意：如果在 Linux 中使用，请确保项目文件夹权限为 1000：1000*

    ```sh
    $ sudo chown -R 1000:1000 $DIANSHAO_YOCTO_PROJECT_PATH
    ```

   *注意：如果在 Windows 中使用，请打开该文件夹大小写敏感选项*

    ```sh
    $ fsutil.exe file setCaseSensitiveInfo $DIANSHAO_YOCTO_PROJECT_PATH enable
    ```

3. Docker 镜像编译
   ```sh
   $ sudo docker-compose build
   ```

   *注意：如果使用国内网络编译速度过慢，可使用如下命令使用国内镜像源*

    ```sh
   $ sudo docker-compose -f docker-compose-with-mirror.yml build
   ```

4. Docker 容器启动
   ```sh
   $ sudo docker-compose up
   ```
   *注意：如果使上一步使用国内镜像源，请使用下面命令*

    ```sh
   $ sudo docker-compose -f docker-compose-with-mirror.yml up
   ```

## 3. 创建项目



## 4. 增加元数据层

## 5. 增加软件包

## 6. 制作镜像文件

## 7. 编译项目

