1、安装
http://docs.basho.com/riak/1.2.1/tutorials/installation/Installing-on-RHEL-and-CentOS/

package=basho-release-6-1.noarch.rpm && \
wget http://yum.basho.com/gpg/$package -O /tmp/$package && \
sudo rpm -ivh /tmp/$package
sudo yum install riak