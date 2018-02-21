---
title: MySQL优化InnoDB磁盘I/O之innodb_buffer_pool_size设置
date: 2017-12-15 21:02:47
tags: MySQL
---

<p>MySQL数据库最重要的一项优化，就是磁盘I/O优化，通常来讲，良好的磁盘I/O优化，能够带来非常显著的性能提升。这里主要描述通过合理利用内存来获得良好的I/O性能，其中一个重要的配置参数就是 innodb_buffer_pool_size 。

1. 根据MySQL官方的文档，从5.5之后，innodb_buffer_pool_size 的默认值为128M，最小可设置为5M。
2. 如果将一台服务器单独作为MySQL来运行，通常建议 innodb_buffer_pool_size 值为总内存的50% ~ 75% 比较适合。
3. 根据官方记载，当innodb_buffer_pool_size设置超过1GB的时候， innodb_buffer_pool_instances 的值则为8，所以建议大于1GB的情况下，设置 innodb_buffer_pool_size 为8的倍数比较友好。

</p>
