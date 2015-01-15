1、日志
[2014-11-12 14:43:22.537] [INFO] hksearch - pad:小米
[2014-11-12 14:46:26.816] [INFO] hksearch - pad:小米
[2014-11-12 14:52:34.944] [INFO] hksearch - pad:大米
[2014-11-12 14:53:32.204] [INFO] hksearch - pad:小米
[2014-11-12 14:57:01.422] [INFO] hksearch - pad:米粉
[2014-11-12 14:57:01.424] [INFO] hksearch - pad:欧莱雅
[2014-11-12 14:57:01.483] [INFO] hksearch - pad:x60s
[2014-11-12 14:57:01.844] [INFO] hksearch - pad:莫斯利安酸奶
[2014-11-12 14:57:02.297] [INFO] hksearch - pad:康佳 42寸
[2014-11-12 14:57:02.638] [INFO] hksearch - pad:苹果6


=================================================================================

2、程序
var fs = require('fs');
var co = require('co');
var redisClient = require('redis').createClient();
var wrapper = require('co-redis');
var redisCo = wrapper(redisClient);
var lazy = require("lazy");

new lazy(fs.createReadStream("hksearch.log"))
.lines
.forEach(function(line){
        var ll                 = line.toString();
        var colums             = ll.split(" ");
        var search_colum       = colums[5];
        var search_key_array   = search_colum.split(":");
        var search_key         = search_key_array[1];

        redisClient.zincrby("rank",1,search_key);

        console.log(search_key);

}).on('pipe',function(){

        console.log("END");

});//End of lazy.....

=================================================================================

3、redis的问题
取中文，使用的是redis-cli --raw这样才能得到非转码的结果
redis-cli --raw

ZREVRANGE rank 1 200


苹果6
华为
iphone
iPhone
手机
茅台
五粮液
平安果
牛栏
花王
小米
iphone6
红酒
牛奶
小米4
大闸蟹
iPhone6

=================================================================================
4、词数
48527

求词频的程序

=================================================================================
5、MYSQL表导入

CREATE DATABASE heike_product;
source /Users/nixiephoebe/search_logs/product_12.12.sql;

=================================================================================
6、MYSQL搜索字段词频计算子程序
完全利用了MYSQL之前的逻辑来COUNT就是了

var mysql = require('mysql');

var pool = mysql.createPool({
    host: '127.0.0.1',
    user: 'root',
    password: '',
    database: 'heike_product',
    port: 3306,
    connectionLimit : 100
});

var selectSQL_product = "select count(PRODUCT_NAME) as ? from product where PRODUCT_NAME like ? OR SEARCHKEY like ?";

var getProduct = function(raw_searchkey,callback){
	var searchkey = "%"+raw_searchkey+"%";
	pool.getConnection(function (err, conn) {
		if (err) console.log("POOL ==> " + err);
			conn.query(selectSQL_product,[raw_searchkey,searchkey,searchkey],function (err2, rows) {
    				if (err2) console.log(err2);
            			for (var i in rows) {
                			callback(rows[i]);
            			}
    			conn.release();
    		});//END of query SQL.......
	});//END of getConnection...
}//END OF getProduct

module.exports.getProduct = getProduct;

=================================================================================

7、输出所有词在商品当中的频度信息的程序

var redisClient = require('redis').createClient();
var tf_counter  = require('./tf_counter.js');
var tf_counter  = tf_counter.tf_counter;


var getKeyWords = function(pageNum,callback){
    var pageSize = 100;
    var start    = pageSize*(pageNum-1);
    var end      = pageSize*pageNum;
    redisClient.ZRANGE("rank",start,end,function(err,range_result){
            callback(range_result);
    });
}//END OF getKeyWords

    //tf_counter("羊腿",function(result){
    //  console.log(result);        
    //});

getKeyWords(760,function(range_result){
    //console.log(range_result);
    for (var i   = range_result.length - 1; i >= 0; i--) {
         var key = range_result[i];
         console.log(key);
            tf_counter(key,function(result){
                console.log(result);        
            });
    };
});