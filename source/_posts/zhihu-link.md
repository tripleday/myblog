title: 基于Neo4j的知乎关系爬虫
date: 2016-06-29 11:15:56
comments: true
tags: 
 - zhihu
 - Neo4j
 - python
categories: Crawler
photos: 
 - /uploads/img/20160629/cover.jpg
---
前两天做了一个爬取知乎用户**follow**关系的爬虫。做这个爬虫是受一个知乎专栏的启发[Web Crawler with Python - 09.怎样通过爬虫找出我和轮子哥、四万姐之间的最短关系](https://zhuanlan.zhihu.com/p/20546546)，我有部分代码参考了xlzd。由于当时也想了解一下NoSQL里Graph Database，于是花了几天时间做了一个简单的爬虫，感觉收获不少。封面图片可以理解成是一个六度分隔理论的直观展现，也是我在做爬虫时的意外验证。
# 环境安装

首先交代一下爬虫所用到的数据库和环境：
## MongoDB
MongoDB一种基于分布式文件存储的数据库，属于NoSQL里的文档型数据库。它的性能较高，面向集合存储，爬虫所抓取的用户信息都存储在其中。
在python里使用MongoDB，只需要在本机下载安装MongoDB服务，在python的环境里安装pymongo依赖，`pip install pymongo`就可以了。如果嫌MongoDB的命令行操作不方便，可以装一个MongoDB的可视化工具[Robomongo](https://robomongo.org/)。

## Neo4j
Neo4j是一个高性能的,NoSQL图形数据库，它将结构化数据存储在网络上而不是表中。Neo4j也可以被看作是一个高性能的图引擎，该引擎具有成熟数据库的所有特性。
关于Neo4j的安装，可以参考这篇博客[Neo4j介绍与使用](http://blog.csdn.net/dyllove98/article/details/8635965)。Win7环境下，官网[下载](https://neo4j.com/download/)可以一键安装Neo4j。
Neo4j使用类似SQL的查询语言**Cypher**，关于Cypher的使用和简单demo，可以参考[Cypher查询语言--Neo4j中的SQL](http://www.uml.org.cn/sjjm/201203063.asp)。当然，为了减少学习Cypher的时间成本，我在python环境中安装了**py2neo**，`pip install py2neo`。py2neo的handbook见[The Py2neo v3 Handbook](http://py2neo.org/v3/)，我对py2neo依赖库的理解是一个Neo4j的客户端，其中对Neo4j的操作进行了封装。调用py2neo的一个函数，它会自动转化为Cypher语言并以HTTP API向Neo4j服务端口提交一个事务。当然它也支持直接提交Cypher语句到Neo4j执行，有些复杂的数据操作比如寻找两点之间最短路径，py2neo没有提供直接的函数调用，需要我们自己编写Cypher。

## python依赖
* requests
request是一个非常好用的网络依赖包，API文档见[Requests: HTTP for Humans](http://www.python-requests.org/en/master/)。文档网站的名字“HTTP for Humans”，算是程序员的一种幽默吧。
* BeautifulSoup
BeautifulSoup依赖库是一个非常实用的HTML解析器，不需要程序员再焦头烂额地写RegEx。虽然开发友好了，但解析时有时会出一些不可思议的bug。API文档见[Beautiful Soup 4.2.0 文档](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/)。

# 爬虫概要
## 目的
我爬虫的目的非常简单，和开头的那篇专栏一样：知乎大V---轮子哥（**vczh**）需要通过多少人才能认识并关注我？这里的认识是指单方面的知道，即成为我的follower（不需要为followee，虽然这是肯定的），知道这世界原来还有个知乎用户“**三天三夜**”。
## 开发思路
爬虫从我自己的知乎出发，读取我的follower列表，对我的每个follower重复搜索操作，直到搜索到的follower list里有vczh。这个遍历是**BFS**的。当然，为了防止在广度优先搜索时，层与层之间节点数量扩张过快，我限制只搜索follower num**不超过100**的不活跃的小用户，当然我提前调查了轮子哥也有follow一些这种小用户。除了为了防止扩张过快导致的存储空间过大，这样做也给验证六度分隔理论提了更为苛刻的条件，毕竟轮子哥通过其他大V能follow到我的概率是远大于通过小用户的。
## 代码相关
整个爬虫的代码我push到Github上，附上[链接](https://github.com/tripleday/zhihu_link)。
贴上几个想到的小细节：
* 这个爬虫需要自己的知乎cookie才能爬取。建议使用chrome，安装**EditThisCookie**插件，将知乎的cookie复制粘贴到zhihu_cookie.json文件。
* 知乎用户的唯一性不是靠用户名，而是html里内嵌隐藏的**data-id**，在ajax获取数据是发送的表单数据里也需要这个值。所以Neo4j中使用此值可以唯一标识用户。
* BeautifulSoup的官方文档里的一张解析器对比表格

![BeautifulSoup解析器对比](/uploads/img/20160629/bs.png)
实际使用中，在解析`https://www.zhihu.com/people/hong-ming-da`这条链接时，lxml解析一直都会出错，换成html.parser后解析成功。所以html.parser虽然解析速度慢，但容错性更好一点。
* 其他的细节想到后再补充。

# 爬取结果

爬虫程序在爬了23928个用户才停下来，即找到了轮子哥。这是爬完的部分用户图：
![部分用户关系图](/uploads/img/20160629/whole.png)

在命令行执行Cypher语句：`MATCH (a {_id : '0970f947b898ecc0ec035f9126dd4e08'}), (b {_id : 'bd648b6ef0f14880a522e09ce2752465'}), p = allShortestPaths( (a)-[*..200]->(b) ) RETURN p`可以得到轮子哥到我的最短路径：
![最短路径图](/uploads/img/20160629/shortestpath.png)

可以发现：轮子哥到我，中间正好经过了6个人。这条路的生成条件是较为严格的。不仅是因为我只选择的小用户进行爬取，而且要知道我的follower目前是只有一个的，轮子哥要连接到我只能通过他。虽然实验得到的 **6** 可能和六度分隔理论恰巧吻合，但鉴于路径选择的苛刻条件，六度的6也许并不只是一种猜想。
