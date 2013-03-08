1.	安装vsftpd
yum install vsftpd

2.	配置用户名和目录
sudo useradd wukongftp -s /sbin/nologin -d /backup/ftp添加用户
sudo passwd wukongftp 用户密码

3.	vsftpd配置
sudo vim /etc/vsftpd/vsftpd.conf
	1. anonymous_enable=NO
	2. chroot_local_user=NO
	chroot_list_enable=YES
	chroot_list_file=/etc/vsftpd/chroot_list

sudo echo wukongftp>/etc/vsftpd/chroot_list

自启动：
sudo /sbin/chkconfig --level 2345 vsftpd on

启动：
sudo /sbin/servicevsftpd start

4.	检查网络
可以尝试在远端登陆一下。

5.	维护脚本
因为对方主动上传数据，需要写脚本去维护ftp的数据，删除太老的数据。
