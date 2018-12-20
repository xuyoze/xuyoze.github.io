---
title: Python3.7下安装scrapy报错解决犯法
date: 2018-12-20 11:05:40
categories: 转载
tags: [scrapy,python]
---
> 升级python 到3.7后, 安装scrapy 遇到报错情况, 网上查询看到如下文章 转载记录

---

在python3.6以上版本安装scrapy框架是会报错缺少Microsoft Visual C++ Build Tools
```
pip install scrapy
```
然后安装会出现下面错误
{% asset_img 20180224171825166.png error %}
<!-- more -->

2.去[www.lfd.uci.edu](http://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted) 下载twisted对应版本的`whl`文件，这里对应版本是只对应你安装的python的版本，寻找你所需要的版本下载下来

3.`pip install` 你下载的whl文件
{% asset_img 2018022417260832.png install %}

4.最后`pip install scrapy`即可
{% asset_img 20180224172808115.png install %}

> 作者：liuzemeeting
来源：CSDN
原文：https://blog.csdn.net/liuzemeeting/article/details/79363981
版权声明：本文为博主原创文章，转载请附上博文链接！