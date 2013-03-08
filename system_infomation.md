1、cpu信息
http://www.cyberciti.biz/faq/linux-get-number-of-cpus-core-command/
a) nproc
b) lscpu
c) less /proc/cpuinfo

2、磁盘信息
df -h
cat /proc/partitions
cat /proc/mounts

3、raid信息

4、内存信息
cat /proc/meminfo
free -m

5、防火墙信息和路由信息
包：iproute
ss
ss -s

6、定位文件
yum install mlocate
updatedb

7、内核以及glibc版本
查看内核版本
cat /proc/version

查看glibc版本
ldd --version

8、硬件信息
rpm -qf `which lspci`
pciutils-3.1.7-3.el5.x86_64

yum install pciutils
lspci

9、系统审核
http://www.tecmint.com/install-lynis-auditing-tool-in-rhel-centos-fedora/

10、SELinux
http://www.centos.org/docs/5/html/Deployment_Guide-en-US/ch-selinux.html

11、系统启动项目
chkconfig --list
/etc/rc.local以及用户目录下的某些东西

12、检查crontab -l