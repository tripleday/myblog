title: Hello World
date: 2016-06-12 21:28:52
comments: true
tags: 
 - English
 - Hexo
categories: Site Est
photos: 
 - /uploads/img/20160612/cover.gif
---

This is my **first blog** after I established this site using [Hexo](https://hexo.io/) and [random](https://github.com/stiekel/hexo-theme-random). 
Here are some tests for Hexo:

# Code Test:
```javascript
$ hexo clean
$ hexo generate
$ hexo server
$ hexo deploy
```
# Mathjax Test:
$$\sum_{i=1}^n a_i=0$$

$$
\begin{eqnarray}
f(x_1,x_2,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2
\end{eqnarray}
$$ 

$$
\begin{eqnarray}
\nabla\cdot\vec{E} &=& \frac{\rho}{\epsilon_0} \\
\nabla\cdot\vec{B} &=& 0 \\
\nabla\times\vec{E} &=& -\frac{\partial B}{\partial t} \\
\nabla\times\vec{B} &=& \mu_0\left(\vec{J}+\epsilon_0\frac{\partial E}{\partial t} \right)
\end{eqnarray}
$$

# PDF Test:
{% pdf http://tripleday.github.io/uploads/pdf/Clean%20Code.pdf %}

# iFrame Test:
{% iframe http://www.seu.edu.cn/english/main.htm 100% 500 %}

# Picture Test:
![Facebook](/uploads/img/20160612/facebook.jpg)

# Youtube Test:
{% youtube https://youtu.be/QBJxGklvHRg %}

# Youku Test:
{% youku 480 %}
XMTU3NjExOTUwMA==
{% endyouku %}

