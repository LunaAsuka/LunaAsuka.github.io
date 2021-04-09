---
title: CTFShow web入门信息收集wp
date: 2021-04-08
description: 信息收集入门
categories:
image:
author_staff_member: LunaAsuka
---

### 简简单单

## web1源码泄露

直接f12就能看到。

## web2绕过前台JS

由于js限制该网页不能直接查看源码，可以考虑禁用js，或者直接Ctrl+U，或者 view-source:http://网站/

## web3协议头信息泄露

响应头里存了flag，直接burp抓包查看，或者f12用network查看。

## web4后台robots泄露

访问robots.txt，可以找到有一个/flagishere.txt目录，直接访问得到flag。

## web5 PHPS源码泄露

直接访问 index.phps，得到源码，就能看到flag。

## web6源码压缩包泄露

有提示，所以可以直接访问。没有提示的情况下可以直接用扫描器扫出来。

解压得到文件，提示flag在fl000g.txt里，直接网页访问即可。

## web7版本控制泄露源码

实际上就是git泄露，可以通过扫描得到。之后可以直接访问或者使用githack这样的工具。

## web8版本控制泄露2

实际上是svn泄露，同理git。也有相关工具可供使用，github自寻。

## web9 vim临时文件泄露

程序员使用vim编辑器编写一个index.php文件时，会有一个.index.php.swp文件，如果文件正常退出，则该文件被删除，如果异常退出，该文件则会保存下来，该文件可以用来恢复异常退出的index.php，同时多次意外退出并不会覆盖旧的.swp文件，而是会生成一个新的，例如.swo文件。

做法同上，直接访问即可。

## web10 cookie泄露

直接查看cookie即可。

## web11域名记录泄露

[http://www.jsons.cn/nslookup/](http://www.jsons.cn/nslookup/)在这个网站里直接查询ctfshow.com即可。

[关于域名解析类型](https://blog.csdn.net/dai451954706/article/details/37696651)

## web12敏感信息泄露

首先扫目录，可以发现有登录口，访问需要账号密码，账号很明显是admin，密码未知，在整个网站内进行搜寻，可以发现页面最下方有一行数字，即为密码，登录拿到flag。![1617869991492](\images\posts\1617869991492.png)

## web13内部技术文档泄露

网页最下方有个document可以点进去，该文件翻到最后可以找到后台地址和账号密码，登录即可。

## web14编辑器配置不当

根据提示，访问editor界面，这里面有上传功能，可以在这里访问目标服务器的内部文件，找到flag即可。

## web15密码逻辑脆弱

扫目录，登录后台，用户名admin，忘记密码，密保答案是西安，至于为啥是西安，你可以找到一个qq群号，这个群显示所在地点是西安。。。然后登录即可。

## web16探针泄露

说了是探针泄露，那就直接访问tz.php，搜索flag即可。

php探针是用来探测空间、服务器运行状况和PHP信息用的，探针可以实时查看服务器硬盘资源、内存占用、网卡 流量、系统负载、服务器时间等信息。 url后缀名添加/tz.php 版本是雅黑PHP探针。

## web17CDN穿透

通过使用各类解析网站或者ping之类的工具得到目标网站的ip即可。

## web18 js敏感信息泄露

根据题目，找js文件，里面有条判断，是当分数达到100以上时触发的

```
if(score>100)
{
var result=window.confirm("\u4f60\u8d62\u4e86\uff0c\u53bb\u5e7a\u5e7a\u96f6\u70b9\u76ae\u7231\u5403\u76ae\u770b\u770b");
}
```

这串Unicode转换成中文就能得到线索。

## web19前段密钥泄露

查看源码，找到了处理后的密码，抓包改密码即可。

## web20数据库恶意下载

 mdb文件是早期asp+access构架的数据库文件，直接查看url路径添加/db/db.mdb，下载文件通过txt打开或者通过EasyAccess.exe打开搜索flag 。