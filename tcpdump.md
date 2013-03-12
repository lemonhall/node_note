1、
写入某文件
tcpdump 'port 80' -w test.cap

读取某文件
tcpdump -r capture.log

显示包内容
tcpdump -A

2、
用TCPDUMP捕获的TCP包的一般输出信息是：
　　src > dst: flags data-seqno ack window urgent options
　　src > dst:表明从源地址到目的地址, flags是TCP包中的标志信息,S 是SYN标志, F (F
IN), P (PUSH) , R (RST) "." (没有标记); data-seqno是数据包中的数据的顺序号, ack是
下次期望的顺序号, window是接收缓存的窗口大小, urgent表明数据包中是否有紧急指针.
Options是选项.


3、seq/ack
应答模式其实很简单

A---->B
===============
这是一个GET / HTTP/1.1的请求，seq号，为1243,ack号为798
Transmission Control Protocol, Src Port: 50290 (50290), Dst Port: http (80), Seq: 1243, Ack: 798, Len: 355

B---->A
===============
接下来就是一个HTTP 200应答，可以看到应答的Seq号即为请求的Ack号......
Transmission Control Protocol, Src Port: http (80), Dst Port: 50290 (50290), Seq: 798, Ack: 1598, Len: 1428


4、相对与绝对
http://seclists.org/tcpdump/2010/q3/21

The other typical problem with reading the structure field is to read it using little-endian byte encoding instead of big-endian (network byte order), but I see you use htonl in your code which is correct.

Try -S option with tcpdump to get "absolute" numbers.

