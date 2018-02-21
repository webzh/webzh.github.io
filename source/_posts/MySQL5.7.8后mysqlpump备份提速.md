---
title: MySQL使用mysqlpump全量备份
date: 2018-02-15 11:02:47
tags: MySQL
categories: 数据库
---
##### mysqlpump介绍 #####
根据官方文档记录，<b>mysqlpump</b>是mysqldump的变种，相比之前的mysqldump全量备份可以使用多线程，以及自带的压缩输出，不指定压缩方式下，默认使用lz4压缩，备份效率大大提升。
根据官方提示:
mysqlpump was added in MySQL 5.7.8. It uses recent MySQL features and thus assumes use with a server at least as recent as mysqlpump itself.
最低版本需要为mysql5.7.8
*** 测试中，导出的总大小约为26G的sql文件数据，利用mysqlpump多线程压缩导出，大概只需要10分钟即可完成。***
<!--more-->

##### 命令使用及说明参数 #####
```shell
# 逻辑备份命令
/usr/local/mysql/bin/mysqlpump -uroot -p
--compress-output=LZ4 --default-parallelism=6 --databases dbname > xxx.sql.lz4

# 关键参数说明：
# --compress-output 压缩输出的压缩算法 可以使用LZ4或者ZLIB
# --default-parallelism 并行导出，即多线程同时导出，提升备份效率的关键参数，值建议不要超过cpu核心数。

# 解压数据, 直接使用mysql自带的解压命令:
/usr/local/mysql/bin/lz4_decompress xxx.sql.lz xxx.sql
```
