title: CentOS 6.5 下scrapy与ghost.py的安装干货
date: 2016-06-16 13:34:08
comments: true
tags: 
 - CentOS
 - Scrapy
 - ghost.py
 - python
categories: Reviews
---
之前一直在做Scrapy中关于网页动态内容的获取，主要目标是想获得javascript渲染后的网页html源码。
在转向使用ghost.py来做脚本解析之前的挖坑爬坑过程中，我已经造访过我所知的大大小小各种论坛、博客以及贴吧和知乎。其中有大方向上指导意义的有知乎里的相关问题：
* [Python 爬虫如何获取 JS 生成的 URL 和网页内容？](https://www.zhihu.com/question/21471960)
* [Python爬虫在处理由Javascript动态生成的页面时有哪些解决方案？](https://www.zhihu.com/question/36450326)

一些前人的技术博客如：
* 开源中国上[斑ban](http://my.oschina.net/u/1024140?ft=blog)的[《使用python，scrapy写（定制）爬虫的经验，资料，杂》](http://my.oschina.net/u/1024140/blog/188154)。
这篇博客里的总结涉及到爬虫的很多方面，看后受益匪浅，作者乃真大神，有很丰富的爬虫经验。

上述几个干货里提到的方法，我基本都去了解了一下，也照着其中的几个方向挖过坑，过些时间我把我在这方面爬的所有坑都总结到一篇博客里。
ghost.py算是我掉坑里时间最长的，也是差点就成功的一个，到现在也弃了，弃的原因日后再说。其实，用ghost.py是在PyQt4的基础上转过去的，ghost.py是对**PyQt4**或者**PySide**的一个封装，需要安装其中一个才能运行。

# PySide
当然挖坑的第一步就是安装环境了，win7上安装简便得多，但到linux下就没那么舒服了。
下面是我在CentOS 6.5和python2.7.11的环境上安装Scrapy、PySide和Ghost.py过程中查到的有用资料的整合。如嫌下面的字太小，可戳此PDF[源地址](http://tripleday.github.io/uploads/pdf/CentOS%26scrapy%26ghost.py.pdf)。
{% pdf http://tripleday.github.io/uploads/pdf/CentOS%26scrapy%26ghost.py.pdf %}
上面的PDF里ghost.py用的是PySide。PySide和PyQt4的功能和API近乎一致，我的理解是：PyQt4是PySide的商业化版本，两者都是Qt进行维护。

# PyQt4
我曾经在用PySide的时候遇到无法解决的Core Dump的bug，想转去试一下PyQt4看会不会好点，虽然结果是bug更频繁，但我还是列出安装PyQt4的一些小tips吧，希望后来人少走点弯路。
安装PyQt4之前是需要安装**SIP**的。SIP是一个自动为C和C++库生成Python扩展模块的工具。为了方便开发PyQt，SIP于1998被“Riverbank Computing”公司创造出来。不过，SIP不专用于PyQt，而是适用于所有的C和C++库。但据说好像现在只有PyQt一直在坚持用SIP，很多别人家的项目在需要对C或C++封装调用的时候都用SWIG了。

在安装过程中最让人抓狂的SIP和PyQt4的版本对应问题：某个固定版本的SIP只能支持少数几个版本的PyQt，有比较麻烦的兼容性问题。曾经在安装时，要么提示SIP版本过高，PyQt无法编译；要么PyQt版本过高，SIP不能支持。
贴一个能够成功安装的博客链接：[CentOS7.1下python2.7.10安装PyQt4](http://blog.csdn.net/dgatiger/article/details/50331361)

文中SIP的安装代码：
```sh
wget http://downloads.sourceforge.net/project/pyqt/sip/sip-4.17/sip-4.17.tar.gz
tar xvf sip-4.17.tar.gz
cd sip-4.17
python configure.py
make & make install & make clean
```
PyQt的安装代码：
```sh
wget http://downloads.sourceforge.net/project/pyqt/PyQt4/PyQt-4.11.4/PyQt-x11-gpl-4.11.4.tar.gz
tar xvf PyQt-x11-gpl-4.11.4.tar.gz
cd PyQt-x11-gpl-4.11.4
python configure.py -q  /usr/lib64/qt4/bin/qmake
make & make install & make clean
```
这篇博客里用的是**sip-4.17**和**PyQt-4.11.4**是能够成功的一对版本。另外，Python2.7最高只能支持到PyQt4，PyQt5好像需要Python3.X的环境；同时Python2与Python3的兼容性也较差。所以为了避免版本上的麻烦，个人建议Python2还是老老实实用PyQt4，Python3的也使用对应的PyQt5。
