---
title: 镜像
weight: 5
---

{{< toc >}}

## 1. 说明

lora gateway 镜像除了基础文件系统之外，主要包括 ec20, packet-forwarder, chirpstack-gateway-bridge 三个软件包

## 2. 创建镜像

我们基于 poky-base 制作文件系统，并选择 sd 作为存储方式

## 3. 镜像制作

我们使用 freescale 官方提供的 wks 文件，因此不需要自己创建 wks 文件

## 4. 文件系统制作

文件系统需要添加 ec20, packet-forwarder, chirpstack-gateway-bridge 三个文件包，同时增加一个 sudo 软件包，增加 admin 用户

## 5. 与上菜配合使用

## 6. 生成镜像、升级包 bbfile

点击 make image file 自动生成镜像 bbfile

## 7. 编译镜像

点击 bitbake image!! 开发编译构建镜像

## 8. 上传升级包
