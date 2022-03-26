---
title: 创建项目
weight: 1
---

{{< toc >}}

## 1. 创建项目

- 项目名为 lora-gateway-imx6ull
- yocto 版本选择 hardknott
- 点击创建，等待项目初始化完成，如果网络不好，时间会较长

## 2. 增加元数据层

计划使用 imx6ull 芯片，因此需要额外添加 freescale 和 freescale-distro 两个元数据层

- meta-freescale: 名称 meta-freescale, url https://github.com/Freescale/meta-freescale.git
- meta-freescale-distro: 名称 meta-freescale-distro, url https://github.com/Freescale/meta-freescale-distro.git

等待元数据层添加完成

## 3. 上传至远端仓库

本项目将作为例程上传至 dianshao-embedded/lora-gateway-imx6ull 仓库中