---
title: Nginx-geoip2模块-限制国家地区访问
date: 2019-03-14 21:22:10
tags: [NGINX, geoip2]
categories: 服务器
---


```shell
cd /usr/local/src
git clone --recursive https://github.com/maxmind/libmaxminddb
cd libmaxminddb
./bootstrap
./configure
make
make install
sh -c "echo /usr/local/lib  >> /etc/ld.so.conf.d/local.conf"
ldconfig

```
