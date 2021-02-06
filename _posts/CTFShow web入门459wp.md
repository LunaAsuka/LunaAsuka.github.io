---
title: CTFShow web入门459wp
date: 2021-02-06
description: 读取日志文件
categories:
image:
author_staff_member: LunaAsuka
---

### 通过copy日志文件getshell

![1612599038831](\images\posts\1612599038831.png)

这道题很简洁，这里用了一个[copy()函数](https://www.php.net/manual/zh/function.copy.php)，一开始我一直纠结在这个函数本身的性质上，翻来覆去的研究半天也看不出个所以然，这个函数的第二个参数是目标文件，如果目标文件存在则直接覆盖，但是如果不存在则会新建一个文件，这是重点，但文档上没有说明，这样就可以构造payload：?u=/var/log/nginx/access,log&p=a

然后直接访问a.php，你会看到copy过来的日志文件，然后就可以改UA头然后通过日志getshell，但是不要忘了，copy过来的a.php并不会产生后续变化，也就是说，你需要先改UA头注入一个<?php eval($_POST['asuka']);?>，然后再去copy日志，鉴于这里已经copy过一次了，所以我们修改完UA并且发包后，重新copy一个到b.php,即?u=/var/log/nginx/access,log&p=b，然后利用蚁剑连上这个b.php，就可以浏览flag.php了。