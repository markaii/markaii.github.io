---
layout: post
title: "构建VIM-IDE最佳实践" 
categories: 工具
tags: vim tool
---

本文是基于Mac环境的配置, OSX自带的vim版本较低, 因此首先安装MacVim

	brew install macvim
	
安装好之后, 将以下配置添加到 `~/.bashrc` 中

	alias vim='/Application/MacVim.app/Content/MacOS/Vim'

## 配置文件

在类UNIX系统中, vim的配置是放在 `~/.vimrc` 文件中, 插件/配色方案/脚本等文件放在 `~/.vim` 目录中的. 但是OSX环境默认没有提供, 需要用以下命令手动添加:

	$ cd ~
	$ touch .vimrc
	$ mkdir .vim
	
### 基本配置

在.vimrc文件中添加以下基本配置

	" 关闭vi兼容,必须放在第一行
	set nocompatible
	 
	" 语法高亮
	if has("syntax")
       syntax on
    endif
   
    " 处理未保存文件时弹出确认
    set confirm
  
    " 黑色背景
    set background=dark
  
    " 配色方案
    colorscheme desert
  
    " 自动对齐
    set autoindent
  
    " 智能对齐
    set smartindent
  
    " 显示行号
    set number
  
    " 打开状态栏标尺
    set ruler
  
    " 显示状态行
    set laststatus=2
  
    " 高亮显示对应的括号
    set showmatch
    
    " 在状态行显示目前所执行的命令，未完成的指令片段亦会显示出来
    set showcmd


### 缩进配置

大部分语言是以4个空格作为标准的缩进的,但是像js这种会频繁出现多重嵌套的语言以2个空格作为标准的缩进,
我希望这个编辑器能够适应不同语言的缩进要求, 将以下配置加入.vimrc中

	" 打开文件类型\插件\缩进检测
 	filetype plugin indent on
  
    " 默认设置4个字符缩进
 	" 设置tab键宽度
    set tabstop=4
  
    set softtabstop=4
    set shiftwidth=4
  
    " 用空格代替tab
    set expandtab
  
    " 设置javascript, xml缩进2个字符
    autocmd FileType javascript,xml set tabstop=2
    autocmd FileType javascript,xml set shiftwidth=2
    autocmd FileType javascript,xml set softtabstop=2


	
## 插件管理

### 安装Vundle管理插件

vim目前主要有两种比较知名的插件管理工具: Vundle && pathogen, 我只使用过Vundle, 所以这里只介绍Vundle的使用方法.

首先, 运行如下命令下载vundle插件到 `~/.vim/bundle/` 目录中
	
	$ cd ~/.vim/bundle
	$ git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim

添加以下设置到.vimrc中

    filetype off
  
    set rtp+=~/.vim/bundle/Vundle.vim/
    call vundle#begin()
  
    " 添加插件的地方
    " let Vundle manage Vundle
    Plugin 'gmarik/Vundle.vim'
  
    call vundle#end()
    filetype plugin indent on

### 使用molokai配色方案

下载配色方案文件

    $ git clone https://github.com/mrtazz/molokai.vim.git

将下载下来的.vim文件放到~/.vim/colors/目录下面
修改.vimrc文件的colorschema为: `colorscheme molokai

配色方案的喜好因人而异, 可以自己去找一些喜欢的配色方案下载下来放到`~/.vim/colors/` 目录中, 然后修改一下.vimrc配置即可.

### 编辑器窗口插件

使用winmanager && nerdtree创建一个如图所示的代码编辑界面:
![window demo](http://ycpub.qiniudn.com/devSnip20150304_3.png)

添加以下配置到.vimrc `call vundle#begin()` 和 `call vundle#end()` 中间
	
	" 窗口管理
    Plugin 'winmanager'
    " 目录管理
    Plugin 'scrooloose/nerdtree'
    Plugin 'scrooloose/nerdcommenter'

保存退出后打开vim运行 `:PluginInstall` 安装

然后在.vimrc的最后添加已下插件设置

	" 启动WinManager
    let g:winManagerWindowLayout='FileExplorer'
    nmap wm :WMToggle<cr>
  
    """"NerdTree配置""""
    " 设置F9快捷键
    map <F9> :NERDTreeToggle<CR>
  
    """"NerdCommenter""""
    "修改<leader>的映射为','
    let mapleader=","  
    
### 项目内查找

使用Grep实现工程内查找, 在.vimrc中加入如下配置
	
	" 查找 
    Plugin 'grep.vim'
打开vim运行 `:PluginInstall` 安装

然后在.vimrc的最后添加已下插件设置
	
	""""Grep F3-工程内查找""""
    nnoremap <silent> <F3> :Grep<CR>

### 自动补全

自动补全使用 `YouCompleteMe`, 在.vimrc中添加下列配置:

	" 自动补全插件YouCompleteMe
    Plugin 'Valloric/YouCompleteMe'
    
保存退出后打开vim输入 `:PluginInstall` 安装

YouCompleteMe的安装和普通插件不太一样, 下载完成之后还需要编译安装:

	cd ~/.vim/bundle/YouCompleteMe
	./install.sh --clang-completer
	
完成之后如果打开vim没有提示YCM未编译, 则说明已经安装完成


