---
layout: single
title: "mongo-express部署" 
categories: 技术
tags: mongodb web admin
---

# 安装

参考[官网](https://github.com/andzdroid/mongo-express)的步骤进行安装配置

# 使用froever-service将系统部署成服务

    $cd PATH/node_modules/mongo-express
    $sudo forever-service install mongo-express -s app.js
    $sudo forever-service delete mongo-express

之后, 使用以下命令:

    $sudo start mongo-express
    $sudo stop mongo-express
    $sudo restart mongo-express
    $sudo status mongo-express
