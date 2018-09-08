---
layout: post
title: linux系统下uwsgi常用参数
date: 2018-09-03
tags: 博客
---


### 前言
* 很多新手在linux系统下使用uwsgi部署项目常常会踩坑，现在为了更好的提高部署项目的效率，就uwsgi中常用的一些参数进行简单说明
---
以下面这个配置为例

        [uwsgi]
        master = true
        processes = 4
        socket = 127.0.0.1:8891 
        chdir = /home/src/aj/
        pythonpath = /home/env/ajenv/bin/python3
        callable = app
        logto = /home/logs/ajuwsgi.log

socket：
 
    uwsgi监听的socket，可以为socket文件或ip地址+端口号(如0.0.0.0:9000)，取决于nginx中upstream的设置

processes：
    
    在app加载前切换到当前目录 

chdir:

    在app加载前切换到当前目录

pythonpath：

    给PYTHONPATH 增加一个目录（或者一个egg），最多可以使用该选项64次。

module：

    加载指定的python WSGI模块（模块路径必须在PYTHONPATH里）

master：

    相当于master=true，启动一个master进程来管理其他进程，以上述配置为例，其中的4个uwsgi进程都是这个master进程的子进程，如果kill这个master进程，相当于重启所有的uwsgi进程

pidfile：

    在失去权限前，将master的pid写到当前文件中

daemonize：

    使进程在后台运行，并将日志打到指定的日志文件或者udp服务器（不会影响nginx日志的输出）
        
