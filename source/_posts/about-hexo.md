---
title: HEXO 中的一些常用方法
date: 2017-12-29 20:37:26
categories: 编程
tags: hexo
disqusIdentifier: fdsF34ff34sd
thumbnailImage: 90c.png
---

这篇文章没有任何意义,只是在联系hexo的一些用法,一记主题的一些配置.
<!-- more -->

<!-- toc -->

## 关于目录

使用关键标签`<!-- toc -->`,可以在指定的地方自动生成目录.

## 图片引入

{% asset_img 90c.png Version %}

## 文字高亮

```
{% hl_text [(classes | hexa code | rgb color | rgba color)] %} 
content
{% endhl_text %}
```
{% hl_text red %}Verizon{% endhl_text %}首席网络工程师兼无线网络负责人{% hl_text green %}Nicola Palmern{% endhl_text %}说：“大规模MIMO是{% hl_text blue %}4G LTE{% endhl_text %}的重要组成部分，并将在5G技术中发挥重要作用，可以降低数十亿个连接中的单位数延迟以及提高他们的可扩展性。”

MIMO，多输入多输出技术（Multiple-Input Multiple-Output）是指在发射端和接收端分别使用多个发射天线和接收天线，使信号通过发射端与接收端的多个天线传送和接收，从而改善通信质量。

这项技术现在使用得最多的是家庭Wi-Fi网络，类型通常是2×2或3×3的天线，即用于发送和接收的两个或三个天线。大规模MIMO采用相同的概念，并将其变成一路。

## 特殊提示框

```
{% alert [classes] %}
content
{% endalert %}
```

{% alert info %}
Verizon没有说明测试使用了多少个天线，但是大规模MIMO通常意味着比现有技术高几个数量级。
{% endalert %}

{% alert success %}
Verizon没有说明测试使用了多少个天线，但是大规模MIMO通常意味着比现有技术高几个数量级。
{% endalert %}

{% alert warning %}
Verizon没有说明测试使用了多少个天线，但是大规模MIMO通常意味着比现有技术高几个数量级。
{% endalert %}

{% alert danger %}
Verizon没有说明测试使用了多少个天线，但是大规模MIMO通常意味着比现有技术高几个数量级。
{% endalert %}

{% alert no-icon %}
Verizon没有说明测试使用了多少个天线，但是大规模MIMO通常意味着比现有技术高几个数量级。
{% endalert %}

## 文字引用块
 hexo 关于引用块的文档[引用块标签](https://hexo.io/zh-cn/docs/tag-plugins.html)

### 普通引用
{% blockquote %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}

### 引用书上的句子
{% blockquote David Levithan, Wide Awake %}
Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.
{% endblockquote %}

### 引用 Twitter

{% blockquote @DevDocs https://twitter.com/devdocs/status/356095192085962752 %}
NEW: DevDocs now comes with syntax highlighting. http://devdocs.io
{% endblockquote %}

### 引用网络上的文章

{% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing %}
Every interaction is both precious and an opportunity to delight.
{% endblockquote %}

## 图片标签
```
{% image [classes] [group:group-name] /path/to/image [/path/to/thumbnail] [width of thumbnail] [height of thumbnail] [title text] %}
```

classes:
- fancybox : Generate a fancybox image.
- nocaption : Caption of the image will not be displayed.
- left : Image will float at the left.
- right : Image will float at the right.
- center : Image will be at center.
- fig-20 : Image will take 20% of the width of post width and automatically float at left.
- fig-25 : Image will take 25% of the width of post width and automatically float at left.
- fig-33 : Image will take 33% of the width of post width and automatically float at left.
- fig-50 : Image will take 50% of the width of post width and automatically float at left.
- fig-75 : Image will take 75% of the width of post width and automatically float at left.
- fig-100 : Image will take 100% of the width of post width.
- clear : Add a div with clear:both; style attached after the image to retrieve the normal flow of the post.

{% image fig-100 http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-15.jpg %}
{% image fig-50  http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-16.jpg %}
{% image fig-50  http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-17.jpg %}
{% image fig-33  http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-18.jpg %}
{% image fig-33  http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-19.jpg %}
{% image fig-33  http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-20.jpg %}

**左右样式**

{% image right fig-75 http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-11.jpg http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-11-560.jpg %}
{% image right fig-25 http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-11.jpg http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-11-560.jpg %}
{% image right fig-25 http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-11.jpg http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-11-560.jpg %}
{% image right fig-25 http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-11.jpg http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-11-560.jpg %}


添加`fancybox`和`groups:[xxx]` 来组成相册

{% image fancybox fig-50 group:cars http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-13.jpg %}
{% image fancybox fig-25 group:cars http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-17.jpg %}
{% image fancybox fig-25 group:cars http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-18.jpg %}
{% image fancybox fig-25 group:cars http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-19.jpg %}
{% image clear fancybox fig-25 group:cars http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-20.jpg %}
<!-- clear 增加一个div-->
{% image fancybox fig-20 group:cars http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-21.jpg %}
{% image fancybox fig-20 group:cars http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-22.jpg %}
{% image fancybox fig-20 group:cars http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-23.jpg %}
{% image fancybox fig-20 group:cars http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-24.jpg %}
{% image fancybox fig-20 group:cars http://d1u9biwaxjngwg.cloudfront.net/tag-plugins-showcase/car-25.jpg %}