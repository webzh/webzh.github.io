---
title: Nginx升级&Websocket配置
date: 2018-12-24 10:51:53
tags: NGINX
categories: 服务器
---
##### 背景 #####
由于项目中需要支持Websocket，当时一股脑直接nginx配置个反向代理到后端的node监听端口。
结果测试中发现报错 websocket nginx failed: Error during WebSocket handshake: 'Connection' header value must contain 'Upgrade'
检查配置没有问题，后来仔细阅读官方文档，发现必须nginx 1.3 版本以上。
官方原文：NGINX has supported WebSocket since version 1.3 and can act as a reverse proxy and do load balancing of WebSocket applications.

#####  nginx平滑升级 #####
基于Centos系统
<p>
1，下载高版本nginx。
```shell
cd /usr/local/src && wget http://nginx.org/download/nginx-1.8.1.tar.gz
```
2, 安装nginx平滑升级
```shell
cd /usr/local/src
tar zxvf nginx-1.8.1.tar.gz && cd nginx-1.8.1
./configure \
--prefix=/usr/local/nginx \
--with-http_ssl_module \
--with-pcre \
--with-zlib= \
--with-openssl= \
--with-http_gzip_static_module \
--with-file-aio \
--with-http_concat_module \
--with-http_footer_filter_module=shared \
--with-http_stub_status_module \
--with-http_sub_module \
--with-http_realip_module \
--with-http_v2_module

make

```
注意这里，不要使用make install
make 完毕之后，将原有的nginx的sbin目录下nginx 移走
假设您的原有的nginx目录为 /usr/local/nginx

```shell

mv /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/old_nginx
# 将新版本的nginx移动到nginx目录下
cp -r /usr/local/nginx-1.8.1/objs/nginx /usr/local/nginx/sbin/

# 执行平滑升级
make upgrade
# 查看 nginx 版本详情
/usr/local/nginx/sbin/nginx -V

```
</p>

#####  nginx反代Websocket配置 #####
也可以参照官方文档来配置，[NGINX as a WebSocket Proxy](https://www.nginx.com/blog/websocket-nginx/)
<!--more-->
```shell
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

upstream websocket {
    server 127.0.0.1:3000;
}

server {
    server_name ws.example.com;
    listen 80;
    location / {
        proxy_pass http://websocket;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
    }
}
```
