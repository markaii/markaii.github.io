---
layout: single
title: "nodejs运行环境搭建(ubuntu)" 
categories: 技术
tags: nodejs tech
---

## nodejs安装

[nodejs](https://nodejs.org/download)使用Linux Binaries安装

    $ wget https://nodejs.org/dist/v4.1.0/node-v4.1.0.tar.gz 
    $ tar -zvxf node-v4.1.0.tar.gz
    $ ./configure
    $ make 
    $ [sudo] make install

## 拉取代码

    $ sudo apt-get install git
    $ git@git.oschina.net:burning007/yiqu_backend.git


## 安装mongodb

参考官网安装指南

后台运行mongod

    sudo service mongod start

同时需要安装mongo-express作为数据库管理界面

## 安装forever后台运行

    npm install -g forever

安装forever-service

    npm install -g forever-service

    sudo forever-service install yiqu -s ./bin/www -e "NODE_ENV=test"
    sudo forever-service delete yiqu
    
    sudo start yiqu
    sudo stop yiqu
    sudo restart yiqu
    sudo status yiqu

## 配置nginx代理

配置nginx, 使其将所有对yiqu.gift的请求转发到localhost:3000

添加yiqu.conf配置文件到`/etc/nginx/site-enabled目录中, 然后重启nginx

## 更新

    git pull
    npm install
    sudo restart yiqu
