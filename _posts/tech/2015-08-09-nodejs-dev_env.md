---
layout: single 
title: "nodejs开发环境搭建" 
categories: 技术
tags: nodejs tech tool
---

## nodejs安装

[nodejs](https://nodejs.org/download)安装推荐从官网下载安装包, mac下载.pkg文件:
![node download](/assets/img/node_download.png)

下载之后, 直接点击安装包安装即可, 之后就可以用npm安装所需的各个模块了

## express安装

[express](http://expressjs.com/)是nodejs常用的web框架, 使用以下命令安装

    $ npm install express --save

## swig模板引擎安装

express默认使用的是[jade](http://jade-lang.com/), 这是一种类似python语法的模板语音, 使用时
需要最后转换为html. 考虑到要增加很多的学习成本和兼容成本, 还是不建议使用jade. 另一个比较常
用的模板[ejs](http://www.embeddedjs.com/)语法比较丑陋, 最终选择swig作为后端模板引擎, swig的
语法和django相近, 博主有较长时间的django开发经历.运行以下命令安装:

    $ npm install swig --save

## coffee script安装

javascript语法丑陋是这门语言饱受诟病的一个重要原因, 现在可以使用[coffee script](http://coffeescript.org/)来写出更加漂亮规范的后端代码了.
运行以下命令安装:

    npm install -g coffee-script

## gulp安装

使用以下命令安装gulp, 安装完成之后可以运行`gulp`命令执行`gulpfile`文件的配置

    npm install -g gulp

## jshint安装
    
[jshint](http://jshint.com)是一个js语法检查插件, 编辑js文件可以自动进行语法检查

    $ npm install -g jshint

另外, 使用vim编辑时可以利用插件实现jshint的功能, vim的通用语法检查插件[Syntastic](https://github.com/scrooloose/syntastic)已支持jshint, 如果想单独使用jshint的插件可以用[jshint.vim](https://github.com/walm/jshint.vim)

## jscs安装

[jscs](http://jscs.info/)也是一个javascript代码检查器, 它设置参考的js编码规范,Syntastic也支持jscs, 运行以下命令安装:

    $ npm install jscs -g
