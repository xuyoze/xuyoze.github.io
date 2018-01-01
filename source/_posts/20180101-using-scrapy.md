---
title: 练习使用scrapy框架抓取图虫图片
date: 2018-01-01 21:14:58
categories: 编程
tags: [python,scrapy]
---
元旦假期，天干物燥，寒风刺骨，不宜外出。黄历上好像是这么说的，嗯， 准没错。

所以在家三天不出屋。。。。

*以上都是借口，其实就是懒，就是宅*

整好在熟悉一把`Python`的爬虫框架`scrapy`。这篇文章不是教程， 只是我在练习过程中出现的一些问题的记录， 如果你想学习`scray`框架的话,请移步其他网站文章.或者直接查看scrapy官方文档.
<!-- more -->

<!--toc-->

## 爬取前准备
https://tuchong.com/explore/
### 爬去入口
https://tuchong.com/rest/tags/%E8%87%AA%E7%84%B6/posts?page=1&count=20&type=hot

## 代码编写

## 相关问题

### 网站有反扒取功能


### 本机没有图片处理相关库

{% alert danger %}
No module named 'PIL'
{% endalert %}

安装Pillow库
```python
pip install pillow
```
[pillow基本知识](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014320027235877860c87af5544f25a8deeb55141d60c5000)

http://scrapy-chs.readthedocs.io/zh_CN/1.0/intro/tutorial.html