svn co http://svn.nginx.com/trunk/
auto/configure
make/install

yum install httpd

ab -n 30000 -c 20 http://127.0.0.1/

wget http://cn2.php.net/get/php-5.4.8.tar.gz/from/hk1.php.net/mirror
tar zxvf php-5.4.8.tar.gz

./configure --with-gd --without-mysql --enable-mbstring --enable-fastcgi --enable-fpm --with-zlib -with-jpeg-dir --with-png-dir --with-iconv
$ <少什么装什么>
$ make && make install
$ cp /etc/php.ini /usr/local/lib/php.ini

配置

$ vi /usr/local/etc/php-fpm.conf
listen.allowed_clients = 127.0.0.1 ;注意取消此处注释，其余配置按需修改

cp /etc/php.ini /usr/local/lib/php.ini

$ vi /usr/local/lib/php.ini
$ vi /etc/nginx/conf.d/testsite.conf
server {
    listen       80;
    server_name  frankekit.org;

    charset utf-8;
    access_log  /var/log/nginx/log/frankekit.access.log  main;
        location / {
                root /var/www/frankekit;
                index index.php;
                try_files $uri $uri/ /index.php?q=$uri&$args;
        }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    location ~ .php$ {
        root           html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /var/www/frankekit$fastcgi_script_name;
        include        fastcgi_params;
    }
    location ~ /.ht {
        deny  all;
    }
}
$ nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
启动

确认域名绑定成功

$ ping frankekit.org
启动服务

$ service nginx restart
$ php-fpm


=======================

Cakephp

==>config/core.php

set DEBUG=>1

find mysql 

mysql_connect(): The server requested authentication method unknown to the client [mysql_old_password]