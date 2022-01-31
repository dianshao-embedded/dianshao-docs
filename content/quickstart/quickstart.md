---
title: 快速开始
weight: 2
---

{{< toc >}}

## 1. 说明

本文将从零开始，让您快速的部署**颠勺**并构建第一个 Yocto 项目。

本文主要目的是让您快速体验**颠勺**，希望在短时间内，可以说服您这个项目对嵌入式 Linux 项目开发是非常有用，可以提供生产力的。

总的来说，颠勺是 Bitbake 的拓展，目的是帮助开发者更方便的开发 Yocto 项目

如果你对 Yocto 很熟悉，那你会很快掌握颠勺。如果你是一个初学者，颠勺将帮助你快速理解和学会开发 Yocto 项目。但是建议你最好有一定的嵌入式 Linux 开发经验。

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

*注意：创建项目时需要拉取 Bitbake 等项目苍鹭，请确保您的网络畅通*

输入项目名称并选择 Yocto 版本号，然后点击创建

![create-project-screenshot](/dianshao-docs/images/create_project.png)

等待项目初始化完成，初始化时间根据你的网络情况，可能会很长，请耐心等待

![project-initial-screenshot](/dianshao-docs/images/project_initial.png)

如果初始化成功，页面如下所示

![success-initial-screenshot](/dianshao-docs/images/success_init.png)

## 4. 增加元数据层

初始化完成的 Yocto 项目中只包含核心层，如果你需要增加其他元数据层，请点击 *Add Therd-Party MetaLayer* 按钮

输入元数据层名称并且选择导入方式，*remote* 方式意味这你想导入的层目前不在该 Yocto 项目主目录中，*local* 意思则相反。如果你选择 *remote*, 你需要输入 *Url*. 

*sub* 意义为：如果你想导入的层是某个 Git 仓库的子目录， 例如 **meta-openembedded/meta-oe/**，那你需要在 *sub* 中填写 meta-oe

![addlayer-screenshot](/dianshao-docs/images/addlayer.png)

等待进程结束，你会发现新添加的元数据层已经在列表中了

![after-addlayer-screenshot](/dianshao-docs/images/after_addlayer.png)

## 5. 测试 Bitbake 命令

你可以在 *bitbake command* 界面中操作 bitbake，可以在该界面中感受颠勺如何操作 bitbake. 

*注意：Bitbake 需要从零构建，对网络环境和计算资源要求很高，如果你使用普通个人电脑和国内网络，第一次编译的时间可能很长，请耐心*

![bitbake-test-screenshot](/dianshao-docs/images/bitbake_test.png)

## 6. 增加软件包

*注意：颠勺自动创建的 Yocto 项目中，默认使用 qemux86-64 作为待开发机器，并默认使用 Systemd 作为启动引导。您可按照 Yocto 开发说明对 machine 和 distro 进行修改，颠勺暂时不提供该部分支持。*

进入刚刚创建的项目后，点击 **DEVELOP MY META** 或者点击顶栏中的 **MyPackages** 进入 MyPackage 界面

点击 **NEW PACKAGES** 创建新的 Yocto Package. 目前颠勺提供对 C/C++ 项目以及 Golang 项目的支持。当然这并不是说您无法增加其他语言的软件包，只是您需要非常了解如何制作这些语言软件包的 BBFILE 文件。

在本例程中，我们随机选择了一个 C 语言项目，[makefile-example](https://github.com/remonbonbon/makefile-example). 由于为 C 语言项目，因此创建时参数应按如下填写。

![package-add-screenshot](/dianshao-docs/images/package_add.png)

完成创建后，点击该软件包右边的 DETAIL 按钮进入详情，在详情中填写该软件包仓库地址、源码路径等参数，这些均为创建 Yocto 包的必要宏定义，您可以去 Yocto 使用说明中查看其具体含义。上述项目填写如下

![package-detail-screenshot](/dianshao-docs/images/package_detail.png)

填写完成后，点击 Update Package 按钮，更新包详情。然后点击 ADD ORIGINAL TASK 添加编译任务，该项目编译任务为 **oe_runmake**，任务类型选择为 COMPILE，子类型选 NONE，这些参数意义请见用户手册中的 MyPackage 部分，如下图所示。

![create-compile-task-screenshot](/dianshao-docs/images/create_compile_task.png)

然后再点击 ADD INSTALL TASK，我们将编译出来的可执行文件放置到终端固件文件系统中的 /usr/bin 文件夹下，如下图所示。

![create-install-task-screenshot](/dianshao-docs/images/create_install_task.png)

完成上述操作后，点击 GENERATE BB/BBAPPEND FILE 即可自动生成 Yocto 包。您可以点击 Bitbake It 按钮测试编译该包。

## 7. 制作并编译镜像文件

终端项目的最终目的是完成 u-boot，内核，文件系统的编译和制作，并将其打包为镜像文件。为了方便项目的裁剪，同一个项目里可以有多个镜像，每个镜像中可以有不同的文件系统，但是 u-boot 和内核只允许存在一个。

yocto 已经提供了两个基础文件系统，我们只需在其基础上增加自己的软件包即可，创建镜像完成后，我们加入刚才增加的makefile-example 包，并点击 Make Image 按钮制作镜像文件。如下图所示

![image-create-screenshot](/dianshao-docs/images/image_create.png)

完成镜像制作后，我们在该镜像界面点击 Bitbake Image 按钮编译该镜像。第一次编译的时间会很长，请保持耐心。

## 8. 项目上传

颠勺已经为您自动初始化了 git 仓库，并增加了 git submodule，后续项目上传仓库按照 git 使用说明使用即可。



