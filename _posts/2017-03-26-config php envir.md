---
layout: post	
tag: PHP Apache MySQL
date: 2016-03-26 16:00:00.000000000 +09:00
---

>- Windows下搭建（Apache+PHP+MySQL）=>WAMP
>- Linux下搭建（Apache+PHP+MySQL）  =>LAMP

配置方式一般有两种：*套件安装*  以及 *自定义安装*。可以从网上搜索wamp套件一次性安装，不过这种方式灵活性不够高。而我下面详细介绍的是**Windows下自定义安装**。

★建议将这几款软件安装到同一个文件夹中，便于管理；另外，每安装完一个软件，便进行测试是否安装成功。

安装顺序为：Apache→PHP→MySQL 

-------


#1.Apache安装：

官网下载链接:  [点击此处下载](http://httpd.apache.org/docs/current/platform/windows.html#down)

![](https://raw.githubusercontent.com/ai-heng/practice/master/Markdownphotos/1.png)

下面有几个下载链接，可以自行选择。我选用了第二种方式进行下载。

![](https://raw.githubusercontent.com/ai-heng/practice/master/Markdownphotos/2.png)

再根据自己的电脑配置选择32位或者64位。
下载完成后，将文件解压。
接下来，打开conf目录下的http.conf文件（可以利用editplus打开，如果没有的话可以从网上自行下载安装），**查找** `Ctrl+F`
```
ServerRoot "C:/Apache24"
```
将文件目录名修改为你的安装位置，例如我安装到了如下位置：

![](https://raw.githubusercontent.com/ai-heng/practice/master/Markdownphotos/3.png)

所以我应该修改为：
```
D:/phpenvir/Apache2.4.25
```
这里需要注意一点：目录斜杠的符号应该是'/'或者'\\\'。
接着，继续 **查找**
```
#
DocumentRoot "c:/Apache24/htdocs"
<Directory"c:/Apache24/htdocs">
#
```
与上面的方法类似，根据自己的安装位置自行修改为：
```
#
DocumentRoot "D:/phpenvir/Apache2.4.25/htdocs"
<Directory"D:/phpenvir/Apache2.4.25/htdocs">
#
```
修改好之后保存文件。
接下来，利用**「管理员身份」**（这里一定要看清楚，是通过管理员身份，我之前就是在这里出过错）打开命令提示符，切换到Apache目录下的bin目录，执行安装程序 httpd -k install.

![](https://raw.githubusercontent.com/ai-heng/practice/master/Markdownphotos/4.png)

命令会提示你Apache服务安装成功。接着输入 httpd –k start 来启动Apache服务。
测试：在浏览器中输入：http://localhost,如果出现下图页面，则表示Apache安装成功。

![](https://raw.githubusercontent.com/ai-heng/practice/master/Markdownphotos/5.png)

另外，如果你希望在任何目录下都可以运行我们的httpd指令，则需要设置一下环境变量:
（打开「计算机」属性→更改设置→高级→环境变量→PATH→编辑用户变量），将安装的Apache完整的bin目录填写进去，末尾加“;”就行了。

-------

# 2.PHP安装：

Apache安装好之后，在下载PHP开发软件之前，先向httpd.conf文件中写入PHP支持模块。
打开httpd.conf文件， **查找**
```DirectoryIndex index.html```
将其修改为
```
#修改首页面文件类型支持
DirectoryIndex index.html index.htm index.php
```
然后，在文件尾部添加模块：
```

#让Apache支持PHP
LoadModule php7_module "D:/phpenvir/php7.1.2/php7apache2_4.dll"	
#告诉Apache php.ini的位置
PHPIniDir  "D:/phpenvir/php7.1.2"	
AddType application/x-httpd-php .php .html .htm
```
配置好之后，开始下载PHP开发工具。

官网下载链接：[点击此处下载](http://php.net/downloads.php)

![](https://raw.githubusercontent.com/ai-heng/practice/master/Markdownphotos/7.png)

从图中看到有「Non Thread Safe」和「Thread Safe」两个版本，下载「Thread Safe」版的PHP环境。

![](https://raw.githubusercontent.com/ai-heng/practice/master/Markdownphotos/8.png)

下载完成之后将其解压到指定文件夹
D:/phpenvir/php7.1.2
然后，将php7.1.2目录下的php.ini-production文件重命名为php.ini，用editplus打开，**查找：**
```;extension_dir="ext"```
将其修改为：
```
extension_dir="D:/phpenvir/php7.1.2/ext"
```
再**查找：**
```;extension=php_mysqli.dll```
将前面的分号去掉。之后，根据开发的不同需求，可以去掉各种扩展前面的分号，如下（前面不带分号的即为自己启用的扩展功能）：
```
;extension=php_bz2.dll
extension=php_curl.dll
;extension=php_fileinfo.dll
;extension=php_ftp.dll
extension=php_gd2.dll
;extension=php_gettext.dll
;extension=php_gmp.dll
;extension=php_intl.dll
;extension=php_imap.dll
;extension=php_interbase.dll
;extension=php_ldap.dll
extension=php_mbstring.dll
;extension=php_exif.dll      ; Must be after mbstring as it depends on it
extension=php_mysqli.dll
;extension=php_oci8_12c.dll  ; Use with Oracle Database 12c Instant Client
;extension=php_openssl.dll
;extension=php_pdo_firebird.dll
;extension=php_pdo_mysql.dll
;extension=php_pdo_oci.dll
;extension=php_pdo_odbc.dll
;extension=php_pdo_pgsql.dll
;extension=php_pdo_sqlite.dll
;extension=php_pgsql.dll
;extension=php_shmop.dll
```
**测试**：
在Apache目录下的htdocs文件夹中新建Index.php文件，填入以下代码：
```php
<?php
    phpinfo();
?>
```
保存之后，在浏览器中输入：http://localhost/index.php
如果出现如下页面，则证明PHP安装成功。

![](https://raw.githubusercontent.com/ai-heng/practice/master/Markdownphotos/9.png)

------

# 3.MySQL安装：
官网下载链接：[点击此处下载](https://dev.mysql.com/downloads/mysql/)

![](https://raw.githubusercontent.com/ai-heng/practice/master/Markdownphotos/10.png)

![](https://raw.githubusercontent.com/ai-heng/practice/master/Markdownphotos/11.png)

你会发现这里有两个版本「msi」和「zip」.
msi格式的可以直接点击安装，按照给出的提示进行安装。Zip格式的需要自己解压，然后进行配置，才能够使用。
我个人下载安装的是「msi」格式的，安装方式比较简单，根据提示一步步安装即可，这里我便不再多做赘述，可以参考以下两个链接。

>- MySQL安装教程（msi格式）：
http://jingyan.baidu.com/article/67662997305dcd54d51b84d4.html
- MySQL安装教程（zip格式）：
http://jingyan.baidu.com/article/f3ad7d0ffc061a09c3345bf0.html

★参照以上两种方式，将MySQL安装完成以后，PHP环境配置就大致完成了，需要注意的一点是平时写的php文件要放在Apache目录下的htdocs文件夹下，例如：D:/phpenvir/Apache2.4.25/htdocs文件夹。如果想要自定义一个存放文件夹，还需要在httpd.conf中进行修改，具体操作可以参考网上的方法。

★以下是我在配置环境过程中参照的两个教程：

>- PHP环境搭建：https://segmentfault.com/a/1190000003409708
>- PHP环境搭建：https://segmentfault.com/a/1190000005119691
