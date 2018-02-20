---
title: PHP实时推送数据至前端
date: 2018-02-20 11:49:44
tags: [PHP,Javascript,NodeJS]
---
<p>
最近的Web项目，需要对用户进行一些实时数据的推送，服务端是NGINX+PHP，无法直接与网页建立实时的通讯。

通过一顿Google倒腾，最后得出两个方法：
1. 利用NGINX的一个pushstream模块 [Github地址](https://github.com/wandenberg/nginx-push-stream-module)
2. 通过NodeJS作为与前端websocket连接，PHP通过redis消息队列来实现数据的传递。


通过对比，利用NGINX的推送模块，会有一些问题，首先该模块比较久远，后续维护较少，且配置后，在使用过程中，无法传递复杂的数据，如json对象。对中文数据的支持也不是很友好，调试起来也不是很方便。
最终选择Websocket + NodeJS + Redis消息队列 + PHP的方案。
<!--more-->
##### 实现原理
- 利用redis消息队列作为中间件，php推送数据到redis队列。
- NodeJS监听redis消息队列，并利用websocket与前端保持连接。
- 通过监听事件触发，将数据传递至客户端。

##### Demo代码分享
- [PHP-push-to-NodeJs](https://github.com/webzh/PHP-push-to-NodeJs)

#### 其他补充
- 代码中实现了简单的服务端推送消息至客户端，如果需要传递如json数据。可以通过NodeJS的<code>JSON.parse()</code>方法来完成json字符串对象的转换，这样传递至客户端即为一个对象数据。
</p>
