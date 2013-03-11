https://help.github.com/articles/set-up-git



1、
git config --global user.name "Lemon Hall"
git config --global user.email "lemonhall@gmail.com"







2、如果是简单clone下来的东西
git push时遇到
git push
error: The requested URL returned error: 403 while accessing https://github.com/lemonhall/tianjin.git/info/refs

fatal: HTTP request failed

用一下网址来解决：
http://stackoverflow.com/questions/7438313/pushing-to-git-returning-error-code-403-fatal-http-request-failed

3、建立秘钥体制
ssh-keygen -t rsa -C "lemonhall@gmail.com"


4、在audits.wukong.com下最好是用ssh的那个来push，push的方法是
git remote set-url origin git@github.com:lemonhall/tianjin.git

所以说，在git clone的时候，最好也选择ssh的地址来clone



