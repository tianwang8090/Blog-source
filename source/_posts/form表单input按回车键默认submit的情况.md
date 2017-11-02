---
title: form表单input按回车键默认submit的情况
date: 2017-11-02 10:28:48
categories:
tags:
icon: fa-form
---
[TOC]
## 一、先说事件处理的几种方式（no jQuery）
### 1.事件属性
```html
<!-- 事件句柄接收的参数中，事件对象只能为'event'，'this'保留字为事件DOM本身 -->
<input type="text" onkeydown="foo(event, this)">
<script>
  function foo(eventObj, eventTarget) {
    console.log(eventObj);
    console.log(eventTarget);
  }
</script>
```
### 2.javascript添加属性
通过js添加的事件会覆盖已经存在的事件。(类似一个属性的修改)
```html
<input type="text" id="a">
<script>
  document.getElementById("a").onkeydown = function (e) {
    console.log(e);
    console.log(this);
  }
</script>
```
### 3.javascript事件监听
通过`addEventListener()`添加的s事件不会覆盖已存在到的事件；通过`removeEventListener()`可以移出添加的事件
```html
<input type="text" id="a">
<script>
  // 所有主流浏览器，除了 IE 8 及更早 IE版本
  document.getElementById("a").addEventListener("keydown", function (e) {
    console.log(e);
    console.log(this);
  })

  // IE 8 及更早 IE 版本
  document.getElementById("a").attachEvent("onkeydown", function (e) {
    console.log(e);
    console.log(this);
  })
</script>
```

## 二、重现情况
### 情况1.只有一个input：input回车会submit
```html
<form action="//baidu.com">
  <input type="text">
</form>
```

### 情况2.一个或多个input和一个或多个button：input回车会submit
```html
<form action="//baidu.com">
  <input type="text" value="test1">
  <input type="text" value="test2">
  <button>btn1</button>
  <button>btn2</button>
</form>
```
注意：form中button默认type=‘submit’，所以点击button会submit

### 情况3.一个或多个input和一个或多个input(type='submit')：input回车会submit
```html
<form action="//baidu.com">
  <input type="text" value="test1">
  <input type="text" value="test2">
  <input type="submit" value="btn2">
  <input type="submit" value="btn3">
</form>
```
由2和3可知：form中下面三种表单控件（按钮）是等效的
- `<input type="submit" value="btn">` 
- `<button>btn</button>` 
- `<button type='submit'>btn</button>` 

## 三、重现总结
在input中回车会submit：
1. 如果form中只有一个input；
2. 如果form中有submit类型的按钮；

## 四、解决方法
### 对于情况1
1. 添加隐藏的input：
```html
<form action="//baidu.com">  
  <input type="text" value="test">
  <!-- 通过style设置隐藏 -->
  <input type="text" value="" style="display: none">
  <!-- 通过设置type='hidden'是无效的 -->
  <!-- <input type="hidden"> -->
</form>
```
2. 阻止input回车默认事件
```html
<form action="//baidu.com">  
  <input type="text" value="test" onkeydown="fun(event)">
</form>

<script>
  function fun (e) {
    if (e && e.keyCode === 13){
      e.preventDefault();
      // return false; 无效
    }
  }
</script>

<!-- 以下等效 -->

<form action="//baidu.com">  
  <input id="input_test" type="text" value="test">
</form>
<script>
  document.getElementById("input_test").onkeydown = function(e) {
    if (e && e.keyCode === 13){
      e.preventDefault();
      // return false; 有效
    }
  }
</script>

<!-- 以下等效 -->

<form action="//baidu.com">  
  <input id="input_test" type="text" value="test">
</form>
<script>
  document.getElementById("input_test").addEventListener("keydown", function(e) {
    if (e && e.keyCode === 13){
      e.preventDefault();
      // return false; 无效
    }
  })
</script>
```
3. 阻止form的默认submit提交事件
```html
<form action="//baidu.com" onsubmit="fun(event)">  
  <input type="text" value="test">
</form>

<script>
  function fun (e) {    
    e && e.preventDefault();
    // return false; 无效
  }
</script>

<!-- 以下等效 -->

<form action="//baidu.com" id="form_test">  
  <input type="text" value="test">
</form>

<script>
  document.getElementById("form_test").onsubmit = function(e) {
    e.preventDefault();
    // return false; 有效
  };
</script>

<!-- 以下等效 -->

<form action="//baidu.com" id="form_test">  
  <input type="text" value="test">
</form>

<script>
  document.getElementById("form_test").onsubmit = function(e) {
    e.preventDefault();
    // return false; 有效
  };
</script>
```
### 对于情况2和情况3
如果采用阻止默认的submit事件（如下），即使点击btn3也不会submit了，**这不是我们想要的。**
```html
<form action="//baidu.com" id="form_test">
  <input type="text" value="test1">
  <input type="text" value="test2">
  <button>btn1</button>
  <button>btn2</button>
  <button type="submit">btn3</button>
</form>

<script>
  document.getElementById("form_test").onsubmit = function(e) {
    e.preventDefault();
    // return false; 等效
  }
</script>
```
方法：给form里面的所有input的onkeydown事件取消默认操作
```html
<form action="//baidu.com" id="form_test">
  <input type="text" value="test1">
  <input type="text" value="test2">
  <button>btn1</button>
  <button>btn2</button>
  <input type="submit" value="submit">
</form>

<script>
  var inputList = document.getElementById("form_test").getElementsByTagName("input");
	Array.prototype.forEach.call(inputList, function(obj) {
    obj.addEventListener("keydown", function(e) {
      e.preventDefault();
      //return false; 无效
    })
  });
</script>
```

## 四、总结
1. 给按钮添加type属性，明确意图，可以避免跳坑
2. 尽量用ajax提交表单信息，尽量避免form提交
3. 通过上面的比较，阻止事件中`e.preventDefault()`相比`return false`在上述所有情况下均适用。
