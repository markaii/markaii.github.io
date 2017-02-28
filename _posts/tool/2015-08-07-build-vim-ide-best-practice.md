---
layout: single 
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

    " 设置javascript, html, css, less, sass, coffeescript, json, xml缩进2个字符
    autocmd FileType javascript,html,htm,css,less,sass,coffee,json,xml set tabstop=2
    autocmd FileType javascript,html,htm,css,less,sass,coffee,json,xml set shiftwidth=2
    autocmd FileType javascript,html,htm,css,less,sass,coffee,json,xml set softtabstop=2


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
修改.vimrc文件的colorschema为: `colorscheme molokai`

配色方案的喜好因人而异, 可以自己去找一些喜欢的配色方案下载下来放到`~/.vim/colors/` 目录中, 然后修改一下.vimrc配置即可.

### 编辑器窗口插件

使用winmanager && nerdtree创建一个如图所示的代码编辑界面:
![window demo](/assets/img/winmanager.png)

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

### 安装ctags

mac自带安装了ctags, 但是使用时会出问题, 需要用brew重新安装

    brew install ctags

安装好之后, 将以下配置添加到 `~/.bashrc` 中

    alias ctags='/usr/local/bin/ctags'

在编辑的项目目录中运行命令创建tags标签文件, vim会自动从当前目录查找

    ctags -R *

### 安装标签列表插件Tagbar

在.vimrc中加入如下配置

    " 标签列表
    Plugin 'tagbar'

打开vim运行 `:PluginInstall` 安装, 然后再.vimrc 添加如下配置

    """"F8打开标签列表""""
    nmap <F8> :TagbarToggle<CR>


### 自动补全 YouCompleteMe

自动补全使用 `YouCompleteMe`, 在.vimrc中添加下列配置:

    " 自动补全插件YouCompleteMe
    Plugin 'Valloric/YouCompleteMe'

保存退出后打开vim输入 `:PluginInstall` 安装

YouCompleteMe的安装和普通插件不太一样, 下载完成之后还需要编译安装:

    cd ~/.vim/bundle/YouCompleteMe
    git submodule update --init --recursive
    ./install.sh --clang-completer

完成之后如果打开vim没有提示YCM未编译, 则说明已经安装完成.
如果安装完之后运行vim崩溃, 并出现以下提示:

    Vim: Caught deadly signal ABRT
    Vim: Finished.

请参考[这里](https://github.com/Valloric/YouCompleteMe/issues/8)解决
安装成功后在.vimrc中添加如下配置:

    """"YouCompleteMe配置""""
    let g:ycm_error_symbol = '>>'
    let g:ycm_warning_symbol = '>*'
    nnoremap <leader>gl :YcmCompleter GoToDeclaration<CR>
    nnoremap <leader>gf :YcmCompleter GoToDefinition<CR>
    nnoremap <leader>gg :YcmCompleter GoToDefinitionElseDeclaration<CR>


### 代码检查插件 Syntastic

[syntastic](https://github.com/scrooloose/syntastic)是一个代码检查的插件, 通过Vundle安装它, 在.vimrc中添加:

    " 代码检查插件 syntastic
    Plugin 'scrooloose/syntastic'

保存退出后打开vim输入 `:PluginInstall` 安装, 安装成功后在.vimrc的最后添加配置:

    set statusline+=%#warningmsg#
    set statusline+=%{SyntasticStatuslineFlag()}
    set statusline+=%*

    let g:syntastic_always_populate_loc_list = 1
    let g:syntastic_auto_loc_list = 1
    let g:syntastic_check_on_open = 1
    let g:syntastic_check_on_wq = 0

### 代码片段补齐工具 ultisnips

[ultisnips](https://github.com/SirVer/ultisnips)是一个代码片段补齐神器, 它
和 `YouCompleteMe`相互集成. 在.vimrc中添加如下代码斌运行 `:PluginInstall` 安装

    " Track the engine.
    Plugin 'SirVer/ultisnips'

    " Snippets are separated from the engine. Add this if you want them:
    Plugin 'honza/vim-snippets'

安装完成后在.vimrc的最后添加配置:

    " Trigger configuration. Do not use <tab> if you use https://github.com/Valloric/YouCompleteMe.
    let g:UltiSnipsExpandTrigger="<c-e>"
    let g:UltiSnipsJumpForwardTrigger="<c-b>"
    let g:UltiSnipsJumpBackwardTrigger="<c-z>"

    " If you want :UltiSnipsEdit to split your window.
    let g:UltiSnipsEditSplit="vertical"

### 引号配对补全

    " 引号配对补全
    Plugin 'Raimondi/delimitMate'
    " for python docstring ", 特别有用
    au FileType python let b:delimitMate_nesting_quotes = ['"']
    " 关闭某些类型文件的自动补全
    "au FileType mail let b:delimitMate_autoclose = 0'"']

### html/xml标签配对补全
    
    " html/xml标签配对补全
    Plugin 'docunext/closetag.vim'
    let g:closetag_html_style=1

### 其他配置

    " less支持
    Plugin 'groenewege/vim-less'
    
    " coffee script支持
    Plugin 'kchmck/vim-coffee-script'


### .vimrc文件

最后附上我的[.vimrc](https://github.com/markaii/vim-conf)文件
