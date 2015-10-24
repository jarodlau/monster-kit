
```
(编写此文时使用的软件版本为BugFree2.1.3+XAMPP-win32-1.7.7)

1、下载XAMPP,到其官方网站下载最新版本(.7z或.zip的免安装版最好)。
2、XAMPP安装在机器上，假定缺省安装到c:/xampp
3、下载 BugFree (http://www.bugfree.org.cn/)
4、把 BugFree 解压缩到 c:/xampp/htdocs
5、在c:/xampp/htdocs/bugfree2/Include中，将Config.Default.php拷贝一份为 Config.inc.php。
6、启动XAMPPControlPanel，检查Apahce和MySQL处于 “Running”的状态
7、访问 http://localhost/bugfree
8、根据提示，安装数据库->安装bugfree
9、安装完成



安装完成第一次登陆可能会遇到各样的错误"Warning: Call-time pass-by-reference has been deprecated in E:\Program Files\EasyPHP 3.0\www\bugfree\Include\Class\XmlParse.class.php on line 46"
解决办法：修改文件php/pho.ini，allow_call_time_pass_reference = On，重启Apache
```