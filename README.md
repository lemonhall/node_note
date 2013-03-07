#【目录】
一、前言
二、收集系统信息
三、安装node.js
四、安装Nginx
五、配置nginx以及Node.js，express.js，以及socket.io


# 一、【前言】

# 二、【收集系统信息】

## 0、收集系统信息的目的

cpu信息直接关系到nginx的配置，磁盘信息多少关系到你的文件存储，raid信息用来搞定系统稳定性问题，内存信息决定了你的应用之后要不要上redis来做缓存，memcached的配置等等....
随着这篇文章的深入，可能还要把node与redis，node与memcached，测试，系统调优等等一系列的东西全部加进来。

## 1、cpu信息
http://www.cyberciti.biz/faq/linux-get-number-of-cpus-core-command/
a) nproc
b) lscpu
c) less /proc/cpuinfo

## 2、磁盘信息
df -h

## 3、raid信息

## 4、内存信息

# 三、【安装Node.js】

## 1、确认操作系统版本
yum install vim redhat-lsb -y
lsb_release  -a

## 2、确认python版本
python -V
一定要大于2.6

## 3、安装编译包
yum install make gcc gcc-c++

## 4、下载源码包,./confiugre,make,make install

## 5、增加用户(cat /etc/group来查看有哪些组)
http://www.cyberciti.biz/faq/howto-linux-add-user-to-group/

useradd -m -d /home/node node
usermod -a -G nobody node

## 6、su node后git部署文件...

## 7、forever start server.js

## 8、把forever start server.js加入启动项里去....
http://hectorcorrea.com/blog/running-a-node-js-web-site-in-production-a-beginners-guide

https://gist.github.com/thehunmonkgroup/2042409

http://drupalcode.org/sandbox/frans/1524436.git/blob/ad7b1a99bf3aefcda353d64a800f5e56f5543e45:/nodejs/node_modules/forever/examples/initd-example

```bash
#!/bin/bash
#
# initd-example      Node init.d 
#
# chkconfig: 345 80 20
# description: Node init.d example
# processname: node
# pidfile: /var/run/initd-example.pid
# logfile: /var/log/initd-example.log
#

# Source function library.
. /lib/lsb/init-functions

NAME=initd-example                  # Unique name for the application
PORT=1234                           # Port (in this case the application uses process.env.PORT to set the port)
INSTANCE_DIR=/var/www/$NAME         # Location of the application source
SOURCE_NAME=main.js                 # Name os the applcation entry point script

user=apache
pidfile=/var/run/$NAME.pid
logfile=/var/log/$NAME.log
forever_dir=/var/run/forever        # Forever root directory.

node=node
forever=forever
awk=awk
sed=sed

start() {
    echo "Starting $NAME node instance: "

    if [ "$id" = "" ]; then
        # Create the log and pid files, making sure that the target use has access to them
        touch $logfile
        chown $user $logfile

        touch $pidfile
        chown $user $pidfile

        # Launch the application
        start_daemon
            $forever start -p $forever_dir --pidfile $pidfile -l $logfile -a -d $INSTANCE_DIR $SOURCE_NAME
        RETVAL=$?
    else
        echo "Instance already running"
        RETVAL=0
    fi
}

restart() {
    echo -n "Restarting $NAME node instance : "
    if [ "$id" != "" ]; then
        $forever restart -p $forever_dir $id
        RETVAL=$?
    else
        start
    fi
}

stop() {
    echo -n "Shutting down $NAME node instance : "
    if [ "$id" != "" ]; then
        $forever stop -p $forever_dir $id
    else
        echo "Instance is not running";
    fi
    RETVAL=$?
}

getForeverId() {
    local pid=$(pidofproc -p $pidfile)
    $forever list -p $forever_dir | $sed -e 's/\x1b\［0-9; ]*m//g' | $awk "\$4 == \"$pid]\" { gsub(/[\[\］/, \"\", \$1); print \$1; }"
}
id=$(getForeverId)

case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    status)
        status -p ${pidfile}
        ;;
    restart)
        restart
        ;;
    *)
        echo "Usage:  {start|stop|status|restart}"
        exit 1
        ;;
esac
exit $RETVAL
```

#四、【安装NGINX】

## 1、下载并安装依赖包
http://articles.slicehost.com/2009/2/2/centos-installing-nginx-from-source

wget http://nginx.org/download/nginx-1.3.14.tar.gz

sudo yum install pcre-devel zlib-devel openssl-devel

## 2、编译安装./configure make install

## 3、增加系统配置文件
http://articles.slicehost.com/2009/2/2/centos-adding-an-nginx-init-script

vim /etc/init.d/nginx

```bash
#!/bin/sh
#
# nginx - this script starts and stops the nginx daemin
#
# chkconfig:   - 85 15 
# description:  Nginx is an HTTP(S) server, HTTP(S) reverse \
#               proxy and IMAP/POP3 proxy server
# processname: nginx
# config:      /usr/local/nginx/conf/nginx.conf
# pidfile:     /usr/local/nginx/logs/nginx.pid

# Source function library.
. /etc/rc.d/init.d/functions

# Source networking configuration.
. /etc/sysconfig/network

# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0

nginx="/usr/local/nginx/sbin/nginx"
prog=$(basename $nginx)

NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf"

lockfile=/var/lock/subsys/nginx

start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    echo -n $"Starting $prog: "
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}

stop() {
    echo -n $"Stopping $prog: "
    killproc $prog -QUIT
    retval=$?
    echo
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
}

restart() {
    configtest || return $?
    stop
    start
}

reload() {
    configtest || return $?
    echo -n $"Reloading $prog: "
    killproc $nginx -HUP
    RETVAL=$?
    echo
}

force_reload() {
    restart
}

configtest() {
  $nginx -t -c $NGINX_CONF_FILE
}

rh_status() {
    status $prog
}

rh_status_q() {
    rh_status >/dev/null 2>&1
}

case "$1" in
    start)
        rh_status_q && exit 0
        $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest)
        $1
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    force-reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart|try-restart)
        rh_status_q || exit 0
            ;;
    *)
        echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
        exit 2
esac
```

chmod +x /etc/init.d/nginx

【Chkconfig】

Now we have the base script prepared, we need to add it to the default run levels:

sudo /sbin/chkconfig nginx on
Let's check our work to confirm:

sudo /sbin/chkconfig --list nginx
nginx           0:off   1:off   2:on    3:on    4:on    5:on    6:off
Done.

The script will now be called on a reboot so Nginx will automatically start.


# 五、【配置nginx以及Node.js，express.js，以及socket.io】

https://engineering.gosquared.com/optimising-nginx-node-js-and-networking-for-heavy-workloads
http://savanne.be/articles/deploying-node-js-with-systemd/
http://blog.argteam.com/coding/hardening-node-js-for-production-part-2-using-nginx-to-avoid-node-js-load/

```
location / {
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_set_header X-NginX-Proxy true;
      proxy_pass http://127.0.0.1:3000/;
      proxy_redirect off;
    }
```

可以用servername的方式来在80端口监听多个站点....

```
server {
        listen  80;
        server_name xxx.example.com;
    }
server {
        listen       80;
        server_name  yyy.example.com;
}
```ruby