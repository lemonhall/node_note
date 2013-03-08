1、一般情况下
yum install mysql-server mysql就OK

2、如果需要源代码安装则去官网

3、下载RPM包也可以
rpm -ivh MySQL-client....rpm
rpm -ivh MySQL-server....rpm

4、日志文件
ps aux | grep mysql就可以看到日志文件的配置位置。。

5、配置
/etc/my.cnf
/usr/my.cnf

6、UTF-8
[client]
character-set-server=utf8

[mysqld]
character-set-server=utf8


7、备份某数据库的表结构
mysqldump -u root -p database --opt -q -d > test.sql

mysqldump -uroot -p'123456' dopool_serv_logs --opt  -E -R -q -d > /tmp/merge_dopool_serv_logs.sql
 
-E 导出events
-R 导出存储过程和自定义函数
-q --quick 直接写文件，不会缓冲
-d --no-data 不导出数据

8、导入数据库结构
mysql -p
source filename.sql