---
title: 项目
weight: 2
---

{{< toc >}}

## 1. 创建项目

- 在颠勺中创建一个新项目十分简单，只需输入项目名称和你需要使用的 Yocto 版本号

- 目前颠勺支持从 zeus 到 hardknott 四个 Yocto 版本
- 颠勺默认创建的 Yocto 项目中使用 dianshao distro, 使用 systemd 作为启动方式 

- 颠勺默认创建的 Yocto 项目中包含如下元数据层

![default-layer-screenshot](/dianshao-docs/images/default_layer.png)

*注意: 由于国内网络环境问题，元数据层仓库拉取时间可能会很长，请耐心等待*

## 2. 增加元数据层

- 默认创建的项目中只包含基本的元数据层，用户可以根据项目需要在项目创建完成后手动增加元数据层
  
- 点击项目详情里的 ADD META LAYER 即可手动增加
- 增加时需要填写名称、仓库路径、远程或者本地、子路径
- 其中仓库路径仅在选择远程拉取时需要填写
- 子路径主要是为了适配多个元数据层在一个代码仓库的情况，例如 meta-openembedded
- 颠勺会自动拉取仓库，增加 git submodule, 将元数据层添加至 Yocto 配置文件

## 3. 上传项目

- 项目默认使用 git 进行项目管理
  
- 颠勺在创建项目时会默认初始化 git 本地仓库，增加 git submodule, 用户仅需 add, commit, push 即可上传项目至 git 远程仓库中

## 4. 本地项目路径

生成的 yocto 项目还需要后续的开发，例如下一章节说的底层开发，因此用户还需要按照传统 yocto 项目的开发方式进行开发。

生成的 yocto 项目位于 $DIANSHAO_YOCTO_PROJECT_PATH, 用户可以使用任意 IDE 打开项目，例如 vscode

## 5. 使用 bitbake 命令行

颠勺最大程度上还原了 bitbake 命令行显示方式，但对于 Error 的显示仍然不如命令行清晰，因此您可以在编译失败时使用命令行查看详细原因

您无需搭建 yocto 编译环境，进入 docker-yocto docker container 即可使用 bitbake 命令

```
docker exec -it dianshao-yocto bash

cd ../yocto/${project_name}

source oe-init-build-env

bitbake target
```