---
title: CSS样式优先级
icon: fa-css3
date: 2017-11-03 15:08:25
categories:
tags:
---
> 平台信息： win 10 64bit, Chrome 62 64bit

## 一、基础的CSS样式优先级

```html
<!DOCTYPE html>
<html lang="en">

  <head>
    <!-- link引入外部样式表，外部样式表的位置就是link标签的位置 -->
    <link rel="stylesheet" href="index.css">
    <!-- style标签内为内部样式表 -->
    <style>
      /* 浏览器会有默认样式，页面上的样式默认会继承浏览器默认样式 */

      /* body是#childId元素的父元素，color属性属于继承属性，子元素会继承父元素的继承属性*/
      body {
        color: blueviolet;
      }
      /* 标签选择器 */
      p {
        color: b;
      }
      /* 类选择器 */
      .childClass {
        color: orangered;
      }
      /* 组合选择器（选择器组合起来，优先级比重相加） */
      p.childClass {
        color: grey;
      }

      /* id选择器 */
      #childId {
        color: orange;
      }
      /* 相同选择器书写顺序：下面覆盖上面，文字颜色会为green */
      #childId {
        color: green;
      }

    </style>
  </head>

  <body>
    <div class="fatherClass" id="fatherId">
      <!-- style属性：行内样式(内联样式) -->
      <p class="childClass" id="childId" style="color: red;">texttext</p>
    </div>
  </body>

</html>
```

## 二、优先级规则总结：

### 优先级权重
| CSS选择器 | 优先级比重（数字越大，优先级越高）：|
| - | - |
| `!important`（慎用） | ∞ |
| 行内样式 | 1000 |
| id选择器 | 100 |
| 类选择器、伪类选择器、属性选择器 | 10 |
| 标签选择器、伪元素选择器 | 1 |

### 优先级顺序
0. !important(慎用)
1. 行内样式(不推荐)
2. id选择器
3. 类选择器、伪类选择器、属性选择器
4. 标签选择器、伪元素选择器
5. 继承
6. 默认样式

### 规则
- 先比较优先级权重，再比较书写位置
- 不同属性会合并
- 相同属性
    1. css书写顺序：下面覆盖上面(link引入的外部样式表的位置就是link标签的位置)
    2. 高优先级覆盖低优先级

## 三、改变优先级

### 1. 改变书写顺序

### 2. 通过组合选择器改变CSS优先级。比如：
| 组合选择器 | 优先级比重 |
| - | - |
| `#childId` | 100 |
| `.childClcass` | 10 |
| `p.childClass` | 1+10=11 |
| `div .childClass` | 1+10=11 |
| `p.childClass.otherClass` | 1+10+10=21 |
| `div.fatherClass .childClass` | 1+10+10=21|
| `div p.childClass` | 1+1+10=12 |
| `#fatherId` | 100 |
| `#fatherId p` | 100+1=101 |
| `div.fatherClass p.childClass` | 1+10+1+10=22|

### 3. `!important`
```css
p.childClass {
  color: grey !important;
}
```
- **在任意css位置使用!important会将该样式优先级提升至最高。（多个!important同样遵从css优先级）**
- **尽量不要使用!important，否则会给后期的维护带来很大困扰。**

### 四、CSS建议
- 非必要时，尽量不要使用!important
- 遵循关注点分离思想，推荐将样式与DOM分开（采用外部样式表或者内部样式表），不推荐书写行内（内联）样式
- 建议用类选择器控制样式，为了方便维护