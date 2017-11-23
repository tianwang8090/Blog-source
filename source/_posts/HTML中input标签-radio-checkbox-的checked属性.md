---
title: 关于HTML中input标签(radio|checkbox)的checked属性
icon: fa-html5
date: 2017-11-23 18:03:47
categories:
tags: html
---
## jQuery操作checked
如果你在开发中需要用jQuery操作单选(radio)或者多选(checkbox)元素的选中与取消选中，那你可能会遇到这样的问题：

假设有这样的一个checkbox:
```html
<input type="checkbox" id="abc"></input>
```
你要用jQuery改变他的选中状态，你可能会这样子写：
```javascript
$("#id").attr("checked", "true");
```
然而你会发现并没有实现期望，经过简单的百度，会有很多文章告诉你用下面的方法：
```javascript
$("#id").prop("checked", "true");
```
然后就成功了。我就是这样过来的。

然后我去看了二者的区别(用谷歌翻译这两句话你会很无助):[jquery官网地址](http://api.jquery.com/category/manipulation/general-attributes/)

- .attr()
> Get the value of an attribute for the first element in the set of matched elements or set one or more attributes for every matched element.
- .prop()
> Get the value of a property for the first element in the set of matched elements or set one or more properties for every matched element.

## 关于`attribute`和`property`
写这篇文章的时候看到一篇译文，讲的正好是他们的区别：[传送门](https://segmentfault.com/a/1190000008781121)，可以去看看，还比较仔细。
总结一下就是：
1. attribute(特性)是HTML标签上的。
> attribute(特性)需要通过getAttribute|setAttribute方法操作。比如常见的class操作，不能`ele.class`出来的。
2. property(属性)属于DOM对象。
> property(属性)类似JS的对象，可以直接`.`或者`[xxx]`操作。

其实对于这些不清晰的我，打开chrome控制台看了一下就明白了：
![控制台图片](/assets/input20171123.png)

