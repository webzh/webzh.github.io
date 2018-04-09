---
title: 日志切割之NGINX
date: 2018-04-09 16:25:31
tags: NGINX
categories: Web架构
---
##### 背景 #####
NGINX默认是没有自动切割日志的功能的，官方简单提供了一个shell代码来处理log。具体见 [NGINX日志处理logrotation](https://www.nginx.com/resources/wiki/start/topics/examples/logrotation/)

#####  实现方法 #####
我们这里用到的是logrotate来实现。
首先检查系统是否有安装 rpm -q logrotate，没有的话，执行安装 yum -y install logrotate 即可.
安装完成后，自动在/etc/cron.daily/下生成个logrotate脚本文件。
而我们需要进入到 /etc/logrotate.d/ 目录，按照logrotate语法规范，设置一个nginx的脚本。
```shell
    cd /etc/logrotate.d/
    vim nginx
    # 输入以下内容，注意日志路径修改成自己服务器对应的实际日志存放路径
    /usr/local/nginx/logs/*.log {
      daily
      dateext
      rotate 10
      missingok
      notifempty
      compress
      sharedscripts
      olddir /data/logs/nginx
      postrotate
          kill -USR1 `cat /usr/local/nginx/logs/nginx.pid`
      endscript
    }
```

##### 计划任务添加  #####
编辑完成后，可以自己测试下。
```shell
   /usr/sbin/logrotate -f /etc/logrotate.d/nginx
```
加入到crontab任务。
```shell
    59 23 * * * /usr/sbin/logrotate -f /etc/logrotate.d/nginx
```
##### 主要参数说明 #####
daily 指定转储周期为每天
weekly 指定转储周期为每周
monthly 指定转储周期为每月
dateext 在文件末尾添加当前日期
compress 通过gzip 压缩转储以后的日志
nocompress 不需要压缩时，用这个参数
notifempty 如果是空文件的话，不转储
olddir 转储后的日志文件放入指定的目录
