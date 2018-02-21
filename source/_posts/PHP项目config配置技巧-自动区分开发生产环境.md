---
title: PHP项目config配置技巧-自动区分开发生产环境
date: 2018-02-21 09:28:40
tags: [PHP, hidef]
categories: Web开发
---
##### 背景 #####
通常一个PHP项目，生成环境跟开发环境config必然是不一样的。例如数据库的配置信息一般不会是一样的。比较老式的做法就是不要将本地的config文件上传到svn或者git仓库中，线上的config由运维单独管理配置。
这种做法会有很多的缺点，项目管理不便，当人多的时候，无法保证每个开发人员都能做到小心不将config文件上传到线上的情况。

##### 环境变量设置 #####
可以在php.ini自定义值，并通过PHP函数获取到。根据不同环境设置不同的值，并以该值来路由config文件，很好的解决了配置文件单独管理的痛点。
<!--more-->
例如：
```shell
vim php.ini
# 增加一行
pro.env=production
```

##### PHP项目代码 #####
修改项目入口文件，我的做法是全局设置一个环境变量常量，以便在项目中任何需要做环境区分的配置或逻辑可以用到。
```php
# 修改项目入口文件index.php 默认环境变量值为develop
$env = ($tmpEnv = get_cfg_var('pro.env')) ? $tmpEnv : 'develop';
//定义当前的环境变量
defined('WEB_ENV') or define('WEB_ENV', $env);

# config目录结构
--config
    |- develop
    |- test
    |- production

# 至于具体如何加载config，这个就根据各自的项目结构不一样自行去适配。
# 例如加载config的db配置文件，通常这样写
require_once('/yourpath/config/' . WEB_ENV . '/db.php');
```
##### 结合hidef隐藏关键配置信息 #####
这个之前的一篇文章介绍过hidef的安装和使用，具体见 [PHP扩展hidef使用](/2018/02/20/PHP替代define扩展hidef使用/)。
例如db配置我们通常可以这样写
```php
# 以下代码涉及的常量，均在hidef.ini中已经预定义了。
$dbConfig = array(
      'mysql_ip' = DB_IP,
      'mysql_port' = DB_PORT,
      'mysql_user' = DB_USER,
      'mysql_password' = DB_PASSWORD
);
```
按照如上设置，这样大家都拉取到生产环境的config也没有关系，没法得知关键信息的，因为关键的信息都统一在线上的hidef.ini中配置的。
