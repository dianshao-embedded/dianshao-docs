---
title: 简介
weight: 1
---

{{< toc >}}

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
![Example image](/static/images/architecture.png)