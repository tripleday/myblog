title: 使用python将图片转化为字符画
date: 2016-07-20 21:35:43
comments: true
tags: 
 - PIL
 - python
categories: Fun
photos: 
 - /uploads/img/20160720/cover.png
---
忽然想玩这个图片转化的把戏，是因为之前在知乎上看到一个专栏里用到了下面这个图：
![拿衣服](/uploads/img/20160720/mo.jpg)
当然，那篇专栏没过几小时就被和谐了。我在网上貌似搜到了这个图片转emoji mosaic的网址，供大家戏耍：[Emoji Mosaic](http://ericandrewlewis.github.io/emoji-mosaic/)。

做这个转emoji马赛克应该蛮复杂的，当然它的源码也很容易找到：[ericandrewlewis/emoji-mosaic](https://github.com/ericandrewlewis/emoji-mosaic)。但我纯属娱乐又不想花功夫，于是就在晚上学了学把图片转为字符图的代码玩玩。

代码很简单：
```python
#-*- coding: utf-8 -*-
from PIL import Image
 
grey2char = ['@','#','$','%','&','?','*','o','/','{','[','(','|','!','^','~','-','_',':',';',',','.','`',' ']
count = len(grey2char)
 
def toText(image_file):
   image_file = image_file.convert('L')# 转灰度
   result = ''# 储存字符串
   for h in range(0,  image_file.size[1]):# height
      for w in range(0, image_file.size[0]):# width
         gray = image_file.getpixel((w,h))
         result += grey2char[int(gray/(255/(count-1)))]
      result += '\r\n'
   return result
 
image_file = Image.open("input.jpg")# 打开图片
image_file = image_file.resize((int(image_file.size[0]), int(image_file.size[1]*0.55)))# 调整图片大小
 
output = open('output.txt','w')
output.write(toText(image_file))
output.close()
```
需要注意的是要安装依赖PIL。

原图：
![](/uploads/img/20160720/input.jpg)

字符图：
![](/uploads/img/20160720/cover.png)
