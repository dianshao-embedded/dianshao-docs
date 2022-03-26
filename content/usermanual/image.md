---
title: 镜像
weight: 5
---

{{< toc >}}

## 1. 说明

颠勺提供 MyImage 功能帮助开发者快速制作文件系统、镜像以及升级包

## 2. 创建

点击 New Image 创建新的镜像，填写必要信息

- Base: 由于文件系统包含太多文件，因此 yocto 提供了两种基础文件系统供我们在其基础上添加自己的文件。其中 minimal 只包含启动所必须的文件，base 则包含了芯片所有外设的固件库
- Flash: 选择终端使用的 Flash 类型，目前提供 spi-nor, rawnand, sd 可选

## 3. 镜像制作

- 目前 Yocto 只支持 sd 卡的镜像制作，用户通过点击 ADD WKS FILE 创建 Wic File 确定分区，yocto 会自动构建wic 镜像
- 主流芯片及开发板厂商通常会在其元数据层中提供默认 wic file，因此也可以不填

## 4. 文件系统制作

文件系统的含义无非就是包含哪些软件及相关文件，而 yocto 包已经将每个软件相关文件编译安装方式完成，因此这里只需填写需要包含哪些 yocto 包，每个包之间用逗号间隔

## 5. 与上菜配合使用

传统方式下，开发者每次编译完成后会手动将镜像烧入终端，进行调试，在频繁修改固件调试时不太方便。

颠勺可以与 [上菜](https://dianshao-embedded.github.io/dishes-docs/) 配合使用，实现开发阶段固件自动更新，方便开发者频繁修改固件测试

- compitable: rauc 固件适配参数设置，请参考 rauc 官方文档说明
- product id: 该终端所属产品在上菜平台上的产品 ID
- fs type: 升级包文件系统类型，例如 ext4
- version: 升级包版本号，在开发阶段，版本号无需正式，用于方便开发者自己区分
- file path: 升级包在 build 文件夹下路径，例如 "build/tmp-glibc/deploy/images/raspberrypi4"
- file name: 升级包名称，例如 "update-bundle-ota-test-image-raspberrypi4.raucb"
- stage: 固件阶段，默认为 dev, 固件阶段定义请参考上菜文档
- dishes url: 上菜平台 url

如果您无需集成上菜进行自动更新，此部分可以略过

## 6. 增加额外宏定义

由于 yocto 功能复杂，上述功能无法 100% 满足其所有场景，因此用户可以通过该功能添加需要的 yocto 宏定义。这些宏定义会写入生成的镜像 bbfile 中

## 7. 生成镜像、升级包 bbfile

点击 make image file 自动生成镜像及 rauc 升级包 bbfile

## 8. 编译镜像

点击 bitbake image!! 开发编译构建镜像，由于 yocto 完全从源码开始编译，加上国内网络较慢，编译时间可能会很长，一两天时间也是正常的

## 9. 上传升级包

点击 dishes image 将升级包上传至上菜平台，由其通知终端进行升级

