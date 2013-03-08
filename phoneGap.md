0、要装XCODE、XCODE command line tools，一大堆前置，另外还需要装brew install iOS-emu

1、下载PhoneGap

2、解压

3、到lib/ios/bin/create
create ~/HelloWorld com.lsl.coco.HelloWorld HelloWorld

4、到HelloWorld目录下

5、www目录是只读的，要保存信息，要用Stroage API...

6、HelloWordld/cordova目录下有build/run两个命令

7、另外，现在Phonegap可以直接作为组件来引用。做好适当得裁剪，或者看看它本身是不是惰性加载的，Apache做得东西就是这么无私啊~

估计还是Adobe觉得没太大价值？

================
LINKS:
1、http://caniuse.com
Can I use it，提供了一个sheet来检查所有的最新特性是否被你所编写的平台所支持

2、http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/
2011年介绍Webkits内核以及游览器渲染方面知识的基础文章

2.1 http://www.chromium.org/developers/design-documents/gpu-accelerated-compositing-in-chrome/
CHROM的官方文档，介绍它自家的GPU加速以及绘图架构

2.2 http://trac.webkit.org/wiki/CoordinatedGraphicsSystem
WebKit的官方介绍

2.3 http://webkithacks.github.com/webkit_presentations/dev-process/#title-slide
Getting Started With Webkit，一个slide talk，提纲挈领

2.4 http://www.vineetgupta.com/2010/11/how-browsers-work-part-1-architecture/
辅助介绍的文章

3、http://www.douban.com/note/263697364/
FOR Javascriptor的VIM配置摘要

3.1、http://www.openvim.com/tutorial.html
一个交互式的VIM教学网站

4、http://www.mobilexweb.com/blog/iphone-5-ios-6-html5-developers
IOS6中HTML5的最新特性

5、http://davidwalsh.name/pointer-events
PhoneGap里需要注意的，一个最新的CSS特性，会影响JS的EVENT

6、http://book.douban.com/subject/20142443/
PhoneGap Mobile Application Development Cookbook
还算不错的PhoneGap官方手册的替代品

7、http://daneden.me/animate/
Animate.css
可以得话，值得一看的CSS Animate库，在Mobile上，尽量写原生的，当然，要善用scss

8、http://metroui.org.ua
Metro UI CSS
Metro UI的HTML5响应式实现

9、http://debug.phonegap.com/
官方的一个调试方案

10、http://www.tegdesign.com/cnn-iphone-app-like-swipe-from-top-menu-using-zepto-js-and-phonegap/

CNN Menu的实现，加上CSS Animate以后效果很好

11、http://www.cnblogs.com/rubylouvre/archive/2011/08/22/2148793.html

requestAnimation的，司徒的文章，由历史讲开去

12、http://www.smashingmagazine.com/2012/12/22/talks-to-help-you-become-a-better-front-end-engineer-in-2013/

smashingMagazine上关于2013年，你如何成为一个更好的前端工程师的一系列Talks的集锦

尤其对于前端的技能的进阶有帮助，推荐重点看关于响应式设计的文章，因为这对于PhoneGap而言，很重要