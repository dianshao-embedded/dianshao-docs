---
title: 软件包
weight: 4
---

{{< toc >}}

## 1. 说明

颠勺提供 MyPakcage 功能帮助开发者快速的创建 yocto 包，包括以下功能

- 自动生成 yocto bb/bbappend file
- 生成并添加包所需额外文件，如配置文件、systemd service 文件等
- 生成并添加软件项目补丁

目前支持 C/C++ golang 两种编程语言软件项目 yocto 包的制作

## 2. 创建

点击 New Package 创建包，填写包必要信息

- Version: 请保持和软件版本相同，用于拉取正确版本软件
- Type: 是 yocto bb 包还是 bbappend 包
- Language: 编程语言是 C/C++ 还是 Golang
- Download Method: 源码仓库下载方式选择
- Initial Method: 软件启动方式选择，目前仅支持 Systemd

## 3. 参数设置

用户需要填写下列参数，颠勺会根据这些参数生成 bbfile

- GORPOXY: 配置 golang 项目编译时的 go module 代理
- Extra Golang Env Variable: 除了 GOROOT GOOS GOARCH GOARM GOCACHE GOPROXY 之外的 GO 环境变量
- Extra Oemake: 对应 yocto 变量 $EXTRA_OEMAKE, 详细说明请参考 yocto 文档
- Source Directory: 源码目录，例如使用 git 方式拉取，就是 $WORKDIR/git
- Depends：包依赖，yocto 概念，详细说明请参考 yocto 文档
- Inherit：包继承，yocto 概念，详细说明请参考 yocto 文档
- Service Auto Enable：systemd 服务自动启动选项
- Service File Name: systemd service 文件名称
- Src Url: 源码仓库下载地址
- Src Revision: 拉取 git 仓库时，如果仓库没有 tag, 则需要填写 commit 版本号
- Src Url Md5: 通过 wget 拉取文件时，需要填写文件 md5 校验
- Src Url Sha256: 通过 wget 拉取文件时，需要填写文件 sha256 校验

## 4. 增加本地文件

构建 yocto 包时经常需要增加本地文件到文件系统，例如 systemd service 文件，例如补丁

- create new files: 增加一个新的文件，点开后，输入文件名以及文件内容
- create patch: 增加一个源码补丁，点开后，输入补丁名称，源码项目中路径，以及修改前后内容

## 5. 增加任务

bbfile 中有 config, build, install 三种任务，用户添加后，颠勺会将其加到 bbfile 中

- add original task: 该功能含义为增加一个 bbfile 原始任务，即颠勺会将该行原封不动的写入 bbfile 中。可选 config, build, install 三种任务，可选 prepend, none, append 三种子类型
- add install task: bbfile 中有较多将文件安装于文件系统某处的 install 任务，使用该功能可以简化任务填写，只需填写文件名，源路径，安装路径，权限

## 6. 增加额外宏定义

由于 yocto 功能复杂，上述功能无法 100% 满足其所有场景，因此用户可以通过该功能添加需要的 yocto 宏定义

## 7. 生成 bbfile

完成上述添加后，点击 Generate bb/bbappend file 生成 yocto 包，包括 bbfile 以及本地文件

用户可以点击 bitbake it! 测试添加的 yocto 包是否正确


