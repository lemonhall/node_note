1、安装开发者工具包
yum groupinstall "Development Tools"

2、安装NTP工具包
yum install ntp
ntpdate tick.gatech.edu

3、安装zlib以及sqlite开发包
yum install zlib zlib-devel sqlite-devel

4、安装rvm，然后自带就安装好了ruby和gem
curl -L https://get.rvm.io | bash -s stable --ruby

5、安装yo grunt以及bower
npm install -g yo grunt-cli bower

6、安装compass否则grunt不正常
gem update --system && gem install compass

Mac 下安装的摘要
==============
https://github.com/yeoman/yeoman/wiki/Manual-Install
说是过时了，可是其实还是很有用，那个官方的最新的Getting Stared页面，掩饰了好多包的依赖

真是无聊的心态

vim ~/.bash_profile

export PATH=/usr/local/bin:/usr/local/sbin:/usr/local/share/npm/bin:/usr/local/share/npm/lib/node_modules/yo/bin:$PATH

source ~/.bash_profile