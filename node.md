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