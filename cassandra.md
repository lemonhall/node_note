1、下载
http://www.apache.org/dyn/closer.cgi?path=/cassandra/2.1.2/apache-cassandra-2.1.2-bin.tar.gz


===================================================================

2、安装JAVA

2.1 安装JDK
wget http://119.254.108.84:8080/jdk.tar.gz
tar zxvf jdk.tar.gz

sudo update-alternatives --install /usr/bin/java java /opt/jdk/bin/java 300
sudo update-alternatives --install /usr/bin/javac javac /opt/jdk/bin/javac 300
sudo update-alternatives --install /usr/bin/jar jar /opt/jdk/bin/jar 300
sudo update-alternatives --config  java

vim /etc/profile
export JAVA_HOME=/opt/jdk
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin


-----------------------------------------------

2.2 安装maven
wget http://mirrors.cnnic.cn/apache/maven/maven-3/3.2.3/binaries/apache-maven-3.2.3-bin.tar.gz

tar zxvf apache-maven-3.2.3-bin.tar.gz
mv apache-maven-3.2.3-bin /opt/maven

vim /etc/profile
export M2_HOME=/opt/maven
export M2=$M2_HOME/bin
export PATH=$M2:$PATH

===================================================================

3、启动
bin/cassandra -f
前台启动

===================================================================

4、启动shell
./cqlsh
Connected to Test Cluster at 127.0.0.1:9042.
[cqlsh 5.0.1 | Cassandra 2.1.2 | CQL spec 3.2.0 | Native protocol v3]
Use HELP for help.
cqlsh>


监听在9042端口

===================================================================

5、CQL查询语言
http://cassandra.apache.org/doc/cql3/CQL.html

2与3之间貌似不兼容

===================================================================
参考文档：http://wiki.apache.org/cassandra/GettingStarted

6、开始一个简单的例子

建立键空间：

CREATE KEYSPACE mykeyspace
WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };

-----------------------------------------------

使用键空间：
USE mykeyspace;

-----------------------------------------------

建立users表：
CREATE TABLE users (
  user_id int PRIMARY KEY,
  fname text,
  lname text
);

-----------------------------------------------

插入数据：
INSERT INTO users (user_id,  fname, lname)
  VALUES (1745, 'john', 'smith');
INSERT INTO users (user_id,  fname, lname)
  VALUES (1744, 'john', 'doe');
INSERT INTO users (user_id,  fname, lname)
  VALUES (1746, 'john', 'smith');

-----------------------------------------------

查询数据：
SELECT * FROM users;

-----------------------------------------------

建立索引并查询：

CREATE INDEX ON users (lname);

SELECT * FROM users WHERE lname = 'smith';

 user_id | fname | lname
---------+-------+-------
    1745 |  john | smith
    1746 |  john | smith

cassandra有二级索引，所以可以这么做，但是因为是基于HASH的索引，所以要做范围查询的时候这么做是否OK？呢？

再说。。。


===================================================================

7、客户端问题

http://wiki.apache.org/cassandra/ClientOptions

node.js这边有且只有一个选择，今天是：2014-12-10号，就是官方驱动
项目地址：https://github.com/datastax/nodejs-driver

mkdir node_cassandra

vim package.json

{
  "name": "node-cassandra",
  "version": "0.0.1",
  "description": "sf express",
  "dependencies": {
  },
  "license": "MIT",
  "repository": {
    "type": "git"
  },
  "readmeFilename": "Readme.md",
  "bugs": {
    "url": "https://github.com/visionmedia/co/issues"
  },
  "homepage": "https://github.com/visionmedia/co"
}

npm install cassandra-driver --save


-----------------------------------------------

客户端提供以下功能：

Node discovery
Configurable load balancing
Transparent failover
Tunability
Paging
Client-to-node SSL support
Row streaming
Prepared statements and query batches

-----------------------------------------------

基础用法：

var cassandra = require('cassandra-driver');
var client = new cassandra.Client({contactPoints: ['host1', 'h2'], keyspace: 'ks1'});
var query = 'SELECT email, last_name FROM user_profiles WHERE key=?';
client.execute(query, ['guy'], function(err, result) {
  assert.ifError(err);
  console.log('got user profile with email ' + result.rows[0].email);
});

-----------------------------------------------

日志：
client.on('log', function(level, className, message, furtherInfo) {
  console.log('log event: %s -- %s', level, message);
});

