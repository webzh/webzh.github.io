---
title: MySQL-5.7单库多线程主从复制
date: 2018-04-26 15:31:08
tags:
---

参考文章 http://www.unixfbi.com/305.html

需要对Master和Slave机器的配置文件略作修改。

### master机器修改配置 ###
```shell
# slave
slave-parallel-type=LOGICAL_CLOCK
slave-parallel-workers=4
master_info_repository=TABLE
relay_log_info_repository=TABLE
relay_log_recovery=ON
```

### slave机器修改配置 ###
```shell
binlog_group_commit_sync_delay = 20
binlog_group_commit_sync_no_delay_count = 20
```

### 主从同步账号设置 ###
```shell

# 在主库上建立replication权限账号， 供从库使用该账号进行数据同步复制，注意x.x.x.x换成自己的从库服务器IP
GRANT REPLICATION SLAVE ON *.* TO 'rep_slave'@'x.x.x.x' IDENTIFIED BY '123456';

```

### 备份账号权限设置 ###
```shell

# mysqlpump 备份数据账号设定，赋予权限。以下为示例，根据自己情况修改响应的参数。
GRANT SELECT, RELOAD, SHOW DATABASES, LOCK TABLES, EXECUTE, SHOW VIEW, EVENT, TRIGGER ON *.* TO 'backup'@'127.0.0.1' IDENTIFIED BY '123456';

# mysql 只读账号设定，方便查看从库复制状态等。
GRANT SELECT, SHOW DATABASES, LOCK TABLES, REPLICATION CLIENT, SHOW VIEW ON *.* TO 'only_read'@'127.0.0.1' IDENTIFIED BY '123456';

# 为了防止从库数据写入操作，最好在 my.cnf 中配置
read-only = 1

```
