---
title: Nginx设置支持apk和ipa文件下载
date: 2018-03-21 15:32:40
tags: NGINX
categories: 服务器
---
##### 背景 #####
通过NGINX部署apk文件及ipa文件，以提供手机端app下载使用，默认nginx不支持apk及ipa类型文件的下载的。
需要通过修改nginx的配置文件来实现。

#####  实现方法 #####
编辑 /opt/nginx/conf/mime.types 文件，增加如下两行代码即可。
```shell
    application/vnd.android.package-archive apk;
    application/iphone pxl ipa;
```
完毕后重启nginx即可。
