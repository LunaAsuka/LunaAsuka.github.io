---
title: CTFShow萌新计划web1-8
date: 2021-01-31
description: 萌新计划wp
categories:
image:
author_staff_member: LunaAsuka
---

### web1-7

```php+HTML
<html>
<head>
    <title>ctf.show萌新计划web1</title>
    <meta charset="utf-8">
</head>
<body>
<?php
# 包含数据库连接文件
include("config.php");
# 判断get提交的参数id是否存在
if(isset($_GET['id'])){
    $id = $_GET['id'];
    # 判断id的值是否大于999
    if(intval($id) > 999){
        # id 大于 999 直接退出并返回错误
        die("id error");
    }else{
        # id 小于 999 拼接sql语句
        $sql = "select * from article where id = $id order by id limit 1 ";
        echo "执行的sql为：$sql<br>";
        # 执行sql 语句
        $result = $conn->query($sql);
        # 判断有没有查询结果
        if ($result->num_rows > 0) {
            # 如果有结果，获取结果对象的值$row
            while($row = $result->fetch_assoc()) {
                echo "id: " . $row["id"]. " - title: " . $row["title"]. " <br><hr>" . $row["content"]. "<br>";
            }
        }
        # 关闭数据库连接
        $conn->close();
    }
    
}else{
    highlight_file(__FILE__);
}
?>
</body>
<!-- flag in id = 1000 -->
</html>
```

快速浏览一下，需要get一个id并且值为1000，需要绕过的点是intval()函数。

我们来看一下[php Manual](https://www.php.net/manual/zh/function.intval.php)的实例：

```php
<?php
echo intval(42);                      // 42
echo intval(4.2);                     // 4
echo intval('42');                    // 42
echo intval('+42');                   // 42
echo intval('-42');                   // -42
echo intval(042);                     // 34
echo intval('042');                   // 42
echo intval(1e10);                    // 1410065408
echo intval('1e10');                  // 1
echo intval(0x1A);                    // 26
echo intval(42000000);                // 42000000
echo intval(420000000000000000000);   // 0
echo intval('420000000000000000000'); // 2147483647
echo intval(42, 8);                   // 42
echo intval('42', 8);                 // 34
echo intval(array());                 // 0
echo intval(array('foo', 'bar'));     // 1
echo intval(false);                   // 0
echo intval(true);                    // 1
?>
```

这里利用了intval漏洞，我们可以传入一个字符串'1000'，这样就可以绕过了。除此之外要注意，输入的id最后是要拼接到sql语句上的，不过在MySQL中1000与'1000'或者"1000"是等价的。

其他解：

​	这里可以使用多种位运算符来绕过

- 125\<\<3，利用左移运算符，经过计算后可以得到1000，并且在数据库中可以运行
- 680\|320，992\|8等，利用或运算符，经过计算可以得到1000，并且一样可以在数据库中使用
-  0b1111101000 ，使用二进制绕过，同上
- 0x3e8，这是使用16进制绕过，同上
- 992^8，144^888，200^800等，利用异或运算符，同上
- ~~1000，利用取反，两次取反即可绕过，同上
- 加减乘除运算符大家都可以自行尝试，这里就不过多赘述了

以上这些都是绕过intval的方法，后面几道题都是在web1的前提上加了正则过滤，大家请自行判断以上解中哪些可以使用。

另外除了这些还有其他各种非预期解，可以从sql的角度出发去考虑，例如id=id -- 等，有兴趣的可以自行研究。

### web8

这题算是一道梗题，仔细观察web1-8的题目描述可以看出是阿呆对抗黑客入侵的血泪史，在被一次次渗透之后，老板终于将阿呆辞退，于是就有了web8以及web8的描述“ 阿呆熟悉的一顿操作，去了埃塞尔比亚。 ”

```php+HTML
<html>
<head>
    <title>ctf.show萌新计划web1</title>
    <meta charset="utf-8">
</head>
<body>
<?php
# 包含数据库连接文件,key flag 也在里面定义
include("config.php");
# 判断get提交的参数id是否存在
if(isset($_GET['flag'])){
        if(isset($_GET['flag'])){
                $f = $_GET['flag'];
                if($key===$f){
                        echo $flag;
                }
        }
}else{
    highlight_file(__FILE__);
}
?>
</body>
</html> 
```

说道程序员跑路，自然少不了~~经典操作~~删库，于是flag即为 rm -rf /* 

payload：?flag= rm -rf /* 

**经 典 永 流 传**