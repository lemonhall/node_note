1、安装开发者工具包
yum groupinstall "Development Tools"

2、安装NTP工具包
yum install ntp
ntpdate tick.gatech.edu

3、安装zlib以及sqlite开发包
yum install zlib zlib-devel sqlite-devel

4、
curl -L https://get.rvm.io | bash -s stable --ruby

5、安装yo grunt以及bower
npm install -g yo grunt-cli bower

