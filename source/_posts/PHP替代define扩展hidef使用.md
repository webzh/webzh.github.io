---
title: PHP替代define扩展hidef使用
date: 2018-02-20 16:00:43
tags: [PHP, hidef, 扩展]
categories: Web开发
---

##### hidef介绍 #####
根据官方原文翻译的意思大致就是，允许在简单的ini文件中定义用户定义的常量，然后像内部常量一样处理，而没有任何通常的性能损失。
其实真正利用hidef扩展提升define性能，在当下的web项目中，实际意义并不大。
但是可以利用其定义在服务器配置文件中，预定义的常量，可以帮助我们实现一些敏感配置的隐藏，如 数据库配置， 支付的密钥等信息，这个会在下一遍文章介绍到。
这里主要说的hidef扩展的安装与使用，本次描述建立在 <b>CentOS 6.x</b> 系统的基础上说明的。
<!--more-->

##### 安装hidef #####
```shell
# 下载hidef
wget https://pecl.php.net/get/hidef-0.1.13.tgz

# 解压安装
tar zxvf hidef-0.1.13.tgz
cd hidef-0.1.13
/usr/local/php/bin/phpize
./configure --enable-hidef --with-php-config=/usr/local/php/bin/php-config && make && make install
```

##### 使用hidef #####
```shell
cd /usr/local/php/etc

# 在 php.ini 文件中加入以下两行
extension=hidef.so
hidef.ini_path=/usr/local/php/etc/

# 建立hidef文件, 命名必须为 hidef.ini
touch hidef.ini

# 写法为每个常量定义独占一行 规则 类型 常量名 = 常量值
# 目前类型仅支持 浮点型，整型， 字符串  基本能够满足日常需要了
# 示例文件内容
[hidef]
float PIE = 3.14
int MYSQL_PORT = 3306
string MYSQL_USERNAME = online
```

##### PHP中使用 #####
```php
<?php
  echo PIE;
?>
```
