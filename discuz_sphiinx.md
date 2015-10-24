2.安装discuz!X2
(1)安装数据库和Web服务器
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql php5-gd phpmyadmin
(2)安装Zend框架
sudo apt-get install zend-framework
(3)下载Discuz安装包
cd
mkdir discus
wget  http://download.comsenz.com/DiscuzX/2.0/Discuz_X2_SC_UTF8.zip
(4)解压
sudo apt-get install unzip
unzip Discuz\_X2\_SC\_UTF8.zip
(5)在/var/www下新建bbs目录，将前面解压出的upload目录下所有文件复制到 bbs目录中
cp –a upload/**/var/www/bbs/
(6) 给var中的www文件夹777属性，
sudo chmod -R 777 /var/www
(7)打开浏览器，输入：
> http://127.0.0.1/bbs/install/index.php
开始安装discuz**




sudo apt-get install libmysqlclient-dev

./configure --prefix=/usr/local/sphinx --with-mysql=/usr/ --with-mysql-includes=/usr/include/mysql/ --with-mysql-libs=/usr/lib/mysql/ --with-mmseg=/usr/local/mmseg/ --with-mmseg-includes=/usr/local/mmseg/include/mmseg/ --with-mmseg-libs=/usr/local/mmseg/lib/





【转】 Discuz X1.5+Sphinx帖子全文检索功能搭建
http://hi.baidu.com/slip2008/blog/item/28e752317478670aebc4af02.html



/usr/local/sphinx/etc/csft.conf  （30和50行有改动，这样改是不对的）
#############################################################
# Discuz! X1.5 + Coreseek/Sphinx Fulltext Search Config File
# http://blog.2332.cn/files/sphinx/csft.conf
#############################################################

# Posts
source posts
{
> type					= mysql

  1. some straightforward parameters for SQL source types
> sql\_host				= localhost
> sql\_user				= root
> sql\_pass				= toor
> sql\_db					= ultrax
> sql\_port				= 3306	# optional, default is 3306

> sql\_query\_pre			= SET NAMES utf8
> sql\_query\_pre			= SET SESSION query\_cache\_type=OFF

> sql\_query\_pre            = REPLACE INTO sph\_counter \
> > SELECT 1,MAX(pid) FROM pre\_forum\_post


> sql\_query				= \
> > SELECT p.pid AS id,p.tid,p.subject,p.message,t.digest,t.displayorder,t.authorid,t.lastpost,t.special \
> > > FROM pre\_forum\_post AS p LEFT JOIN pre\_forum\_thread AS t USING(tid) \
> > > WHERE p.pid>=$start AND p.pid<=$end
    1. WHERE pid<=(SELECT max\_doc\_id FROM sph\_counter WHERE counter\_id = 1)

  1. ql\_query\_range = SELECT (SELECT MIN(pid) FROM pre\_forum\_post),max\_doc\_id FROM sph\_counter WHERE counter\_id=1

> sql\_query\_range = SELECT MIN(pid),MAX(pid) FROM pre\_forum\_post

> sql\_range\_step          = 4096

> sql\_attr\_uint            = tid
> > sql\_attr\_uint            = digest
> > sql\_attr\_uint            = displayorder
> > sql\_attr\_uint            = authorid
> > sql\_attr\_uint            = special


> sql\_attr\_timestamp      = lastpost

> sql\_query\_info		= SELECT **FROM `pre_forum_post` WHERE pid=$id
}**

source posts\_minute: posts
{
> sql\_query\_pre            = SET NAMES utf8
> sql\_query\_pre            = SET SESSION query\_cache\_type=OFF
  1. ql\_query\_range            = SELECT max\_doc\_id+1,(SELECT MAX(pid) FROM pre\_forum\_post) FROM sph\_counter WHERE counter\_id=1
> sql\_query\_range            = SELECT MIN(pid),MAX(pid) FROM pre\_forum\_post
}

source posts\_merge: posts\_minute
{
> sql\_query\_pre            = SET NAMES utf8
> sql\_query\_pre            = SET SESSION query\_cache\_type=OFF
> sql\_query\_post		= REPLACE INTO sph\_counter SELECT 1,MAX(pid) FROM pre\_forum\_post
}

index posts
{
> source			= posts
> path			= /usr/local/sphinx/var/data/posts
> docinfo			= extern
> mlock			= 0
> morphology		= none
> min\_word\_len		= 2
> charset\_type		= zh\_cn.utf-8
> charset\_dictpath = /usr/local/mmseg/etc/
> html\_strip				= 0
}

index posts\_minute: posts
{
> source			= posts\_minute
> path			= /usr/local/sphinx/var/data/posts\_minute
}

# must merge after run
index posts\_merge: posts
{
> source			= posts\_merge
> path			= /usr/local/sphinx/var/data/posts\_merge
}

# Threads
source threads
{
> type					= mysql

  1. some straightforward parameters for SQL source types
> sql\_host				= localhost
> sql\_user				= root
> sql\_pass				= toor
> sql\_db					= ultrax
> sql\_port				= 3306	# optional, default is 3306

> sql\_query\_pre			= SET NAMES utf8
> sql\_query\_pre			= SET SESSION query\_cache\_type=OFF
> sql\_query\_pre            = REPLACE INTO sph\_counter SELECT 2,MAX(tid) FROM pre\_forum\_thread

> sql\_query				= \
> > SELECT t.tid AS id,t.tid,t.subject,t.digest,t.displayorder,t.authorid,t.lastpost,t.special \
> > FROM pre\_forum\_thread as t \
> > WHERE closed = 0 AND t.tid>=$start AND t.tid<=$end


> sql\_query\_range            = SELECT (SELECT MIN(tid) FROM pre\_forum\_thread),max\_doc\_id FROM sph\_counter WHERE counter\_id=2
> sql\_range\_step          = 4096

> sql\_attr\_uint            = tid
> > sql\_attr\_uint            = digest
> > sql\_attr\_uint            = displayorder
> > sql\_attr\_uint            = authorid
> > sql\_attr\_uint            = special


> sql\_attr\_timestamp        =lastpost

> sql\_query\_info		= SELECT **FROM `pre_forum_thread` WHERE tid=$id
}**

source threads\_minute: threads
{
> sql\_query\_pre            = SET NAMES utf8
> sql\_query\_pre            = SET SESSION query\_cache\_type=OFF
> sql\_query\_range            = SELECT max\_doc\_id+1,(SELECT MAX(tid) FROM pre\_forum\_thread) FROM sph\_counter WHERE counter\_id=2
}

# must merge after run
source threads\_merge: threads\_minute
{
> sql\_query\_pre            = SET NAMES utf8
> sql\_query\_pre            = SET SESSION query\_cache\_type=OFF
> sql\_query\_post 		=  REPLACE INTO sph\_counter SELECT 2,MAX(tid) FROM pre\_forum\_thread
}

index threads
{
> source			= threads
> path			= /usr/local/sphinx/var/data/threads
> docinfo			= extern
> mlock			= 0
> morphology		= none
> min\_word\_len		= 2
> charset\_type		= zh\_cn.utf-8
> charset\_dictpath = /usr/local/mmseg/etc/
> html\_strip				= 0
}

index threads\_minute: threads
{
> source			= threads\_minute
> path			= /usr/local/sphinx/var/data/threads\_minute
}

index threads\_merge: threads
{
> source			= threads\_merge
> path			= /usr/local/sphinx/var/data/threads\_merge
}
#############################################################################
## indexer settings
#############################################################################

indexer
{
> mem\_limit			= 64M
}

#############################################################################
## searchd settings
#############################################################################

searchd
{
  1. hostname, port, or hostname:port, or /unix/socket/path to listen on
  1. multi-value, multiple listen points are allowed
  1. optional, default is 0.0.0.0:3312 (listen on all interfaces, port 3312)
  1. 
> listen				= 0.0.0.0:3312
  1. listen			= /var/run/searchd.sock
> log					= /usr/local/sphinx/var/log/searchd.log
> query\_log			= /usr/local/sphinx/var/log/query.log
> read\_timeout			= 5
> client\_timeout			= 10
> max\_children			= 30
> pid\_file			= /usr/local/sphinx/var/log/searchd.pid
> max\_matches			= 10000
> seamless\_rotate			= 1
> preopen\_indexes			= 0
> unlink\_old			= 1
> mva\_updates\_pool		= 1M
> max\_packet\_size			= 8M
> max\_filters			= 256
> max\_filter\_values		= 4096
}