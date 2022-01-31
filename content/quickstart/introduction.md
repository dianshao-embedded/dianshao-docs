---
title: 简介
weight: 1
---

{{< toc >}}

![颠勺](/dianshao-docs/images/mainpage.png)

## 1. 项目特征

- 一个嵌入式 Yocto Linux 终端项目构建管理工具


- 提供 BBFILE 自动生成功能降低开发 Yocto 项目的难度，**对初学者友好**
- 提供标准化的嵌入式 Linux 项目开发管理流程，提高嵌入式终端项目的可维护性
- 提供 Yocto 项目模板导入导出功能，用标准化的项目模板加速开发
- 项目运行于 Docker 容器之上，可跨平台部署（Linux、Windows），部署简便

## 2. 依赖项目

颠勺主要是基于 Django 开发， 它通过由 Celery + Redis 支持的异步队列发送 bitbake 命令进行编译等操作，另外它使用 Postgresql 作为数据库


为了快速可靠的安装部署，颠勺和相关依赖均运行于 Docker 容器之中

* [Bitbake](https://github.com/openembedded/bitbake)
* [Yocto](https://www.yoctoproject.org/)
* [Django](https://www.djangoproject.com/)
* [Docker](https://www.docker.com/)
* [Postgresql](https://www.postgresql.org/)
* [Celery](https://docs.celeryproject.org/en/stable/)
* [Redis](https://redis.io)
* [Skeleton](http://getskeleton.com/)
* [JQuery](https://jquery.com)

## 3. 系统架构

颠勺主要由 **dianshao-web** 和 **dianshao-yocto** 两部分组成。前者负责提供 UI 交互界面，项目管理，包管理，元数据层管理等工作，后者负责项目创建，BBFILE 创建，Bitbake 命令运行等底层工作。正常工作时，**dianshao-web** 下发任务至 **dianshao-yocto**，并实时读取任务状态。另外，Bitbake 任务进程相关状态值由 **dianshao-yocto** 通过 UDP 报文主动上报给 **dianshao-web**

<br>

![项目架构](/dianshao-docs/images/architecture.png)