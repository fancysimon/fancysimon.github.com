---
layout: post
title: "tmux安装配置"
description: ""
category: "Programming"
tags: [tmux]
---
{% include JB/setup %}


## tmux安装配置

### 下载

tmux依赖libevent 2.0以上版本。
我这里安装的是tmux 1.9a，libevent是2.0.21版本。
下载地址如下：
[http://downloads.sourceforge.net/tmux/tmux-1.9a.tar.gz](http://downloads.sourceforge.net/tmux/tmux-1.9a.tar.gz)
[https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz](https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz)

### 安装libevent

    tar -zxvf libevent-2.0.21-stable.tar.gz 
    cd libevent-2.0.21-stable/
    ./configure
    make clean && make && sudo make install

此时直接安装tmux会报错，提示找不到libevent库，需要做软连接

    ln -s /usr/lib64/libevent-2.0.so.5 /usr/local/lib/libevent-2.0.so.5
    ln -s /usr/lib64/libevent-2.0.so.5 /usr/lib64/libevent.so


### 安装tmux

    tar -zxvf tmux-1.9a.tar.gz
    cd tmux-1.9a
    ./configure
    make clean && make && sudo make install

### 配置tmux

更改默认按键前缀

    set -g prefix ^a
    unbind ^b
    bind a send-prefix

水平或垂直分割窗口

    unbind '"'
    bind - splitw -v # 分割成上下两个窗口
    unbind %
    bind | splitw -h # 分割成左右两个窗口

选择分割的窗格

    bind k selectp -U # 选择上窗格
    bind j selectp -D # 选择下窗格
    bind h selectp -L # 选择左窗格
    bind l selectp -R # 选择右窗格

重新调整窗格的大小

    bind ^k resizep -U 10 # 跟选择窗格的设置相同，只是多加 Ctrl（Ctrl-k）
    bind ^j resizep -D 10 # 同上
    bind ^h resizep -L 10 # ...
    bind ^l resizep -R 10 # ...

交换两个窗格

    bind ^u swapp -U # 与上窗格交换 Ctrl-u
    bind ^d swapp -D # 与下窗格交换 Ctrl-d

执行命令，比如看 Manpage、查 Perl 函数

    bind m command-prompt "splitw -h 'exec man %%'"
    bind @ command-prompt "splitw -h 'exec perldoc -f %%'"

复制粘贴操作
按 C-a [ 进入复制模式，如果有设置 setw -g mode-keys vi 的话，可按 vi 的按键模式操作。移动至待复制的文本处，按一下空格，结合 vi 移动命令开始选择，选好后按回车确认。
按 C-a ] 粘贴已复制的内容

    set-option -g status-keys vi                      #操作状态栏时的默认键盘布局；可以设置为vi或emacs
    set-option -g status-utf8 on                      开启状态栏的UTF-8支持
    set-window-option -g mode-keys vi    #复制模式中的默认键盘布局；可以设置为vi或emacs
    set-window-option -g utf8 on         #开启窗口的UTF-8支持

### 使用

    tmux new -s session
    tmux new -s session -d #在后台建立会话
    tmux ls #列出会话
    tmux attach -t session #进入某个会话
