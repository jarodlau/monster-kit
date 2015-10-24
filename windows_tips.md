
```
[开机自动登录]
rundll32 netplwiz.dll,UsersRunDll
把‘登录需要xxx’去掉，然后提示输入自动登录时使用的用户名和密码，如果使用Administrator那首先先设置一下它的密码会比较安全

[添加'Dos在这里'功能]
把下面的内容保存到xxx.reg里运行就可以了
        Windows Registry Editor Version 5.00
        
        [HKEY_CLASSES_ROOT\*\shell\Open_DOS_Box]
        @="DOS 在这里"
        [HKEY_CLASSES_ROOT\*\shell\Open_DOS_Box\command]
        @="CMD.EXE /K \"cd %L\""
        [HKEY_CLASSES_ROOT\Folder\shell\Open_DOS_Box]
        @="DOS 在这里"
        [HKEY_CLASSES_ROOT\Folder\shell\Open_DOS_Box\command]
        @="CMD.EXE /K \"cd %L\""

[添加'file类型'功能]
首先要安装file for windows，和Linux里的file命令类似
然后类似上面的'Dos在这里'(一点点改动而已)，把下面的内容弄到注册表里
	Windows Registry Editor Version 5.00

	[HKEY_CLASSES_ROOT\*\shell\Open_file_cmd]
	@="file类型"
	[HKEY_CLASSES_ROOT\*\shell\Open_file_cmd\command]
	@="CMD.EXE /K \"file.exe \"%L\"\""
	[HKEY_CLASSES_ROOT\Folder\shell\Open_file_cmd]
	@="file类型"
	[HKEY_CLASSES_ROOT\Folder\shell\Open_file_cmd\command]
	@="CMD.EXE /K \"file.exe \"%L\"\""

[不保存最近打开文档]
运行——gpedit.msc——用户配置——管理模板——任务栏和开始菜单——启用'不保存最近打开文档记录'

[禁止Ping入]
这个办法还没测试过是不是真的好使
运行——gpedit.msc——计算机配置——Windows设置——安全设置——IP安全策略，在本地计算机——客户端(仅响应)

[Google浏览器启动就隐身]
chrome.exe -incognito -start-maximized

[扫描局域网有哪些机器]
nmap.exe -sP 192.168.1.0/24

[对抗P2P终结者]
静态绑定网关的MAC地址即可
arp -s 192.168.1.1 xx-xx-xx-xx-xx-xx

[以别的用户运行程序]
类似Linux的sudo -u
runas /u:UserXXX "C:\Program Files\some_exe.exe"

[mplayer]
暴风影音等商业软件一般都有木马或广告，尽量不要用
播放rm出错：如果是缺少drvc.dll，去www.dlldll.com下一个放到codecs目录下
字幕乱码：在'选项'里设置'语言'为中文，然后选个中文字体

[如何使SMPlayer不保存播放历史]
打开SMPlayer目录下的smplayer.ini(没有就创建一个)，找到[history]一项，修改两个max_items为0即可，如下：
         [history]
         recents=@Invalid()
         recents\max_items=0
         urls=@Invalid()
         urls\max_items=0
         
         另外，[directories]中有保留最后一次打开的目录和DVD目录名，可以清空，如下(貌似不好使)：
         
         [directories]
         latest_dir=
         last_dvd_directory=

[智能ABC]
做为程序杀手：顺序输入v+左键+Delete+回车，当前等待输入的程序立刻挂掉
如果有机密信息，尽量不使用搜狗输入法，感觉会自动上传个人信息

[右键MD5]
Hashtab     http://beeblebrox.org/

[loop filesystem for windows]
步骤说明http://prabhur.wordpress.com/2009/04/28/mount-ext2-loop-file-under-windows-environment/
1. Download and Install ext2 driver for Windows, you can download it from http://www.fs-driver.org/download.html
2. Download filedisk utility from http://www.acc.umu.se/~bosse/index.html
3. Extract the filedisk zip file and copy the driver (chr/i386/filedisk.sys) to %systemroot%\system32\drivers\ and %systemroot%\system32\
4. Double click on filedisk.reg to make registery entries (the filedisk.reg file can be imported if double click doesn’t work)
5. Restart your machine.

Usage
In command prompt go to filedisk directory and use the following commands:
1. filedisk /mount [size[k|M|G] | /ro | /cd]
example: c:\filedisk>filedisk /mount 0 c:\loop\w3serv_01.02.1.loop f:
You can see the loop file mounted as F: drive
2. filedisk /umount
example: c:\filedisk>filedisk /umount f:
3. filedisk /status
example: c:\filedisk>filedisk /status f:
Alternatively by installing “ext2 driver for Windows” is useful to connect any external hard disk with ext2 file system to view it’s contents.
以上是ext2的挂法，其实如果是NTFS或者Fat32就只需要filedisk这个工具就可以了

[squid2.7 statble8 for windows]
http代理上网的简单工具
首先把etc下的squid.conf和mime.conf准备好
然后编辑squid.conf
除了设置http_access allow all以及http_port 3128之外
有时候启动不了可能是因为visible_hostname没设置
如果连接不上可能要设置一下client_netmask或者检查squid所在机器有没有奇怪的防火墙
默认squid只能放在c:\squid目录下，如果想放到别的目录，把squid.conf里的路径配置都改一下
并且启动的时候要指定参数squid.exe -f c:/windows/xxxx/etc/squid.conf
可以注册为系统服务的，有什么不确定的就看squid.exe -h

squid安装
1,解压squid-2.7.STABLE8-bin.zip到C:
2,重命名C:\squid\etc\mime.conf.default为C:\squid\etc\mime.conf
3,拷贝squid.conf到C:\squid\etc目录下
4,打开dos窗口，进入目录C:\squid\sbin
5,执行命令squid.exe -f C:\\squid\etc\\squid.conf -z

squid启动
1,打开dos窗口，进入目录C:\squid\sbin
2,执行命令squid.exe -f C:\\squid\etc\\squid.conf   (默认端口7078)


```