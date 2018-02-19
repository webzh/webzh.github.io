---
title: MySQL使用mysqlpump全量备份
date: 2018-02-15 11:02:47
tags:
    Mysql
#categories:
#    Database
---


<p>
根据官方文档记录，mysqlpump是mysqldump的变种，相比之前的mysqldump全量备份可以使用多线程，以及自带的压缩输出，不指定压缩方式下，默认使用lz4压缩，备份效率大大提升。

根据官方提示:
mysqlpump was added in MySQL 5.7.8. It uses recent MySQL features and thus assumes use with a server at least as recent as mysqlpump itself.
<b>最低版本需要为mysql5.7.8</b>

经过对一台8核CPU进行mysqlpump备份测试。<br />
<b>解压后大约26G的sql文件数据，导出时间大概只需要10分钟即可完成。</b>
<!--more-->
机器配置为8核CPU。下面为备份语句。
<pre>
<code>
/usr/local/mysql/bin/mysqlpump -uroot -p
--compress-output=LZ4 --default-parallelism=6 --databases dbname > xxx.sql.lz4
</code>
</pre>


关键参数说明：<br />
--compress-output 压缩输出的压缩算法 可以使用LZ4或者ZLIB <br />
--default-parallelism 并行导出，即多线程同时导出，提升备份效率的关键参数，值建议不要超过cpu核心数。


解压数据: <br />
直接使用mysql自带的解压命令: <pre><code>/usr/local/mysql/bin/lz4_decompress xxx.sql.lz xxx.sql</code></pre>

</p>
