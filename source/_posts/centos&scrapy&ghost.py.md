title: CentOS 6.5 下scrapy与ghost.py的安装干货
date: 2016-06-16 13:34:08
comments: true
tags: 
 - 中文
 - CentOS
 - Scrapy
 - ghost.py
categories: Reviews
---
之前一直在做Scrapy中关于网页动态内容的获取，主要目标是想获得javascript渲染后的网页html源码。
在转向使用ghost.py来做脚本解析之前的挖坑爬坑过程中，我已经造访过我所知的大大小小各种论坛、博客以及贴吧和知乎。其中有大方向上指导意义的有知乎里的相关问题：
* [Python 爬虫如何获取 JS 生成的 URL 和网页内容？](https://www.zhihu.com/question/21471960)
* [Python爬虫在处理由Javascript动态生成的页面时有哪些解决方案？](https://www.zhihu.com/question/36450326)

一些前人的技术博客如
* 开源中国上[斑ban](http://my.oschina.net/u/1024140?ft=blog)的[《使用python，scrapy写（定制）爬虫的经验，资料，杂》](http://my.oschina.net/u/1024140/blog/188154)。
这篇博客里的总结让人受益匪浅，作者乃真大神，有很丰富的爬虫经验。

上述几个干货里提到的方法，我基本都去了解了一下，也照着其中的几个方向挖过坑，过些时间我把我在这方面爬的所有坑都总结到一篇博客里。
ghost.py算是我掉坑里时间最长的，也是曾给了我巨大希望的一个，虽然现在也弃了，弃的原因日后再说。其实，用ghost.py是在PyQt4的基础上转过去的，ghost.py是对**PyQt4**或者**PySide**的一个封装，需要安装其中一个才能运行。
当然挖坑的第一步就是安装环境了，win7上安装简便得多，但到linux下就没那么舒服了。
下面是我在CentOS 6.5上安装Scrapy、PySide和Ghost.py过程中查到的有用资料的整合，如嫌下面的字太小，可戳此PDF[源地址](http://tripleday.github.io/pdf//CentOS%26scrapy%26ghost.py.pdf)
{% pdf http://tripleday.github.io/pdf//CentOS%26scrapy%26ghost.py.pdf %}

