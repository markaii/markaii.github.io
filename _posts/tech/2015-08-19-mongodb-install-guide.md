---
layout: single
title: "mongodb安装指南(mac)" 
categories: 技术
tags: mongodb tech db
---

# 安装Mongodb

mac上安装主要有两种方式: Homebrew安装 && 手动安装

## 使用Homebrew安装Mongodb

### 1 更新Homebrew

打开系统shell, 运行以下命令

    brew update

### 2 安装Mongodb

使用brew安装Mongodb有以下几种方式, 选择其一安装即可:

**Install Mongodb Binaries**

    brew install mongodb

**Build MongoDB Source with TLS/SSL Support**

    brew install mongodb --with-openssl

**Install the Latest Development Release of Mongodb**

    brew install mongodb --devel


## 手动安装Mongodb

建议只有Homebrew无法使用时才手动安装

### 1 下载最新版Mongodb安装包

**在shell中运行一下命令:**

    curl -O https://fastdl.mongodb.org/osx/mongodb-osx-x86_64-3.0.4.tgz

**加压安装包**

    tar -zxvf mongodb-osx-x86_64-3.0.4.tgz

**复制解压文件到目标文件夹**

    mkdir -p mongodb
    cp -R -n mongodb-osx-x86_64-3.0.4/ mongodb

**PATH环境变量添加Mongodb执行文件位置**

Mongodb执行文件在`bin/`目录中,打开`~/.bashrc`文件, 添加以下命令:

    export PATH=<mongodb-install-directory>/bin:$PATH

使用实际安装位置替换`<mongodb-install-directory>`

# 运行Mongodb

### 1 创建数据目录

首次运行Mongodb之前,需要先创建供Mongodb写入数据的文件目录. 默认使用目录`/data/db`, 如果你创建
了其他目录, 需要在环境变量`dbpath`中指定

**创建默认的`/data/db`目录:

    mkdir -p /data/db

### 2 设置data目录权限

首次运行 `mongod`命令之前, 请确保执行 `mongod` 命令的用户对`db`目录有读写权限

    sudo chmod 777 /data/db

### 运行Mongodb

如果使用默认的数据目录(i.e., /data/db), 只需要在shell中输入`mongod`:

    mongod

**指定data目录**

    mongod --dbpath <path to data directory>

ps: 本文档翻译自Mongodb官方[安装文档](https://docs.mongodb.org/getting-started/shell/tutorial/install-mongodb-on-os-x/)
