## （2）写代码：css div 垂直水平居中，并完成 div 高度永远是宽度的一半（宽度可以不指定）   


```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      * {
        margin: 0;
        padding: 0;
      }

      html,
      body {
        width: 100%;
        height: 100%;
      }

      .outer {
        width: 400px;
        height: 100%;
        background: blue;
        margin: 0 auto;

        display: flex;
        align-items: center;
      }

      .inner {
        position: relative;
        width: 100%;
        height: 0;
        padding-bottom: 50%;
        background: red;
      }

      .box {
        position: absolute;
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner">
        <div class="box">hello</div>
      </div>
    </div>
  </body>
</html>
```


### 参考链接


- [https://github.com/cttin/cttin.github.io/issues/2](https://github.com/cttin/cttin.github.io/issues/2)



## 请你讲一讲 CSS 的权重和优先级


### 权重


- 从0开始，一个行内样式+1000，一个id选择器+100，一个属性选择器、class或者伪类+10，一个元素选择器，或者伪元素+1，通配符+0



### 优先级


- 权重相同，写在后面的覆盖前面的
- 使用 `!important` 达到最大优先级，都使用 `!important` 时，权重大的优先级高



### 参考链接


- [https://zhuanlan.zhihu.com/p/41604775](https://zhuanlan.zhihu.com/p/41604775)
## 问：介绍 Flex 布局，flex 是什么属性的缩写：



- 弹性盒布局，CSS3 的新属性，用于方便布局，比如垂直居中
- flex属性是 `flex-grow`、`flex-shrink` 和 `flex-basis` 的简写



### 参考链接


- [https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html](https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)



## 问：CSS 怎么画一个大小为父元素宽度一半的正方形？


```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .outer {
        width: 400px;
        height: 600px;
        background: red;
      }

      .inner {
        width: 50%;
        padding-bottom: 50%;
        background: blue;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner"></div>
    </div>
  </body>
</html>

```
## CSS实现自适应正方形、等宽高比矩形
> - width 设置百分比
> - padding 撑高
> - 如果只是要相对于 body 而言的话，还可以使用 vw 和 vh
> - 伪元素设置 `margin-top: 100%`撑高

### 双重嵌套，外层 relative，内层 absolute
```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .outer {
        padding-top: 50%;
        height: 0;
        background: #ccc;
        width: 50%;
        position: relative;
      }

      .inner {
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        background: blue;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner">hello</div>
    </div>
  </body>
</html>

```
### padding 撑高画正方形
```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .outer {
        width: 400px;
        height: 600px;
        background: blue;
      }

      .inner {
        width: 100%;
        height: 0;
        padding-bottom: 100%;
        background: red;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner"></div>
    </div>
  </body>
</html>

```
### 相对于视口 VW VH
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .inner {
        width: 1vw;
        height: 1vw;
        background: blue;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner"></div>
    </div>
  </body>
</html>

```
### 伪元素设置 margin-top
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .inner {
        width: 100px;
        overflow: hidden;
        background: blue;
      }

      .inner::after {
        content: "";
        margin-top: 100%;
        display: block;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner"></div>
    </div>
  </body>
</html>

```


### 参考链接


- [http://www.fly63.com/article/detial/2104](http://www.fly63.com/article/detial/2104)
## （2）问：实现两栏布局的方式：


### 左 float，然后右 margin-left（右边自适应）
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      div {
        height: 500px;
      }

      .aside {
        width: 300px;
        float: left;
        background: yellow;
      }

      .main {
        background: aqua;
        margin-left: 300px;
      }
    </style>
  </head>
  <body>
    <div class="aside"></div>
    <div class="main"></div>
  </body>
</html>

```
### 右 float，margin-right
```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      div {
        height: 500px;
      }

      .aside {
        width: 300px;
        float: right;
        background: yellow;
      }

      .main {
        background: aqua;
        margin-right: 300px;
      }
    </style>
  </head>
  <body>
    <div class="aside"></div>
    <div class="main"></div>
  </body>
</html>

```
### BFC + float
```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      div {
        height: 500px;
      }

      .aside {
        width: 300px;
        float: left;
        background: yellow;
      }

      .main {
        overflow: hidden;
        background: aqua;
      }
    </style>
  </head>
  <body>
    <div class="aside"></div>
    <div class="main"></div>
  </body>
</html>

```
### float + 负 margin
```html
<head>
  <style>
    .left {
      width: 100%;
      float: left;
      background: #f00;
      margin-right: -200px;
    }

    .right {
      float: left;
      width: 200px;
      background: #0f0;
    }
  </style>
</head>

<div class="left"><p>hello</p></div>
<div class="right"><p>world</p></div>
```
### 圣杯布局实现两栏布局
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      /* div {
        height: 500px;
      } */

      /* .box {
        overflow: hidden;
      } */

      /* .container {
        padding: 0 300px 0 200px;
        border: 1px solid black;
      } */

      html,
      body {
        height: 100%;
      }

      div {
        height: 100%;
      }

      .container {
        display: flex;
      }

      .content {
        flex: 1 1;
        order: 2;
        background: #f00;
      }

      .left {
        float: left;
        width: 100%;
        background: #0f0;
      }

      .right {
        float: left;
        width: 300px;
        margin-left: -300px;
        background: #00f;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="left">你好</div>
      <div class="right">我好</div>
    </div>
  </body>
</html>

```
### flex 实现两栏布局
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      /* div {
        height: 500px;
      } */

      /* .box {
        overflow: hidden;
      } */

      /* .container {
        padding: 0 300px 0 200px;
        border: 1px solid black;
      } */

      html,
      body {
        height: 100%;
      }

      div {
        height: 100%;
      }

      .container {
        display: flex;
      }

      .content {
        flex: 1 1;
        order: 2;
        background: #f00;
      }

      .left {
        flex: 0 0 200px;
        background: #0f0;
      }

      .right {
        flex: 1 1;
        background: #00f;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="left">你好</div>
      <div class="right">我好</div>
    </div>
  </body>
</html>

```
参考链接：[https://juejin.im/post/5e8d5268f265da480f0f9c6e#heading-8](https://juejin.im/post/5e8d5268f265da480f0f9c6e#heading-8)


### position + margin
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      /* div {
        height: 500px;
      } */

      /* .box {
        overflow: hidden;
      } */

      /* .container {
        padding: 0 300px 0 200px;
        border: 1px solid black;
      } */

      html,
      body {
        height: 100%;
      }

      div {
        height: 100%;
      }

      .container {
        display: flex;
        position: relative;
      }

      .content {
        flex: 1 1;
        order: 2;
        background: #f00;
      }

      .left {
        position: absolute;
        width: 300px;
        background: #0f0;
      }

      .right {
        width: 100%;
        margin-left: 300px;
        background: #00f;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="left">你好</div>
      <div class="right">我好</div>
    </div>
  </body>
</html>

```
## 手写题：实现一个两栏三列的布局，并且要求三列等高，且以内容最多的一列的高度为准。
## 实现三列布局的方式


### position + margin-left + margin-right
```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      div {
        height: 500px;
      }

      .box {
        position: relative;
      }

      .left {
        position: absolute;
        left: 0;
        top: 0;
        width: 200px;
        background: green;
      }

      .right {
        position: absolute;
        right: 0;
        top: 0;
        width: 200px;
        background: red;
      }

      .middle {
        margin-left: 200px;
        margin-right: 200px;
        background: black;
      }
    </style>
  </head>
  <body>
    <div class="box">
      <div class="left"></div>
      <div class="middle"></div>
      <div class="right"></div>
    </div>
  </body>
</html>

```
### 通过 float + margin
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      div {
        height: 500px;
      }

      .left {
        float: left;
        width: 200px;
        height: 200px;
        background: green;
      }

      .right {
        float: right;
        width: 200px;
        height: 200px;
        background: red;
      }

      .middle {
        margin-left: 210px;
        margin-right: 210px;
        background: black;
        height: 200px;
      }
    </style>
  </head>
  <body>
    <div class="box">
      <div class="left"></div>
      <div class="right"></div>
      <div class="middle"></div>
    </div>
  </body>
</html>

```
### 圣杯布局
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .container {
        padding: 0 300px 0 200px;
        border: 1px solid black;
      }

      .content {
        float: left;
        width: 100%;
        background: #f00;
      }

      .left {
        width: 200px;
        background: #0f0;
        float: left;
        margin-left: -100%;
        position: relative;
        left: -200px;
      }

      .right {
        width: 300px;
        background: #00f;
        float: left;
        margin-left: -300px;
        position: relative;
        right: -300px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="content">中间内容</div>
      <div class="left">左侧区域</div>
      <div class="right">右侧区域</div>
    </div>
  </body>
</html>

```
### 双飞翼布局
```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
      }

      div {
        height: 100%;
      }

      .main {
        float: left;
        width: 100%;
        background: #f00;
      }

      .main .content {
        margin-left: 200px;
        margin-right: 300px;
      }

      .left {
        width: 200px;
        background: #0f0;
        float: left;
        margin-left: -100%;
      }

      .right {
        width: 300px;
        background: #00f;
        float: left;
        margin-left: -300px;
      }
    </style>
  </head>
  <body>
    <div class="main">
      <div class="content">hello world</div>
    </div>
    <div class="left">你好</div>
    <div class="right">王鹏浩</div>
  </body>
</html>

```
### flex 布局
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      html,
      body {
        height: 100%;
      }

      div {
        height: 100%;
      }

      .container {
        display: flex;
      }

      .content {
        flex: 1 1;
        order: 2;
        background: #f00;
      }

      .left {
        flex: 0 0 200px;
        order: 1;
        background: #0f0;
      }

      .right {
        flex: 0 0 300px;
        order: 3;
        background: #00f;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="content">hello world</div>
      <div class="left">你好</div>
      <div class="right">王鹏浩</div>
    </div>
  </body>
</html>

```
### 参考链接

- [https://segmentfault.com/a/1190000003942591](https://segmentfault.com/a/1190000003942591)
- [https://blog.csdn.net/crystal6918/article/details/55224670](https://blog.csdn.net/crystal6918/article/details/55224670)
- [https://blog.csdn.net/zhoulei1995/article/details/80161240](https://blog.csdn.net/zhoulei1995/article/details/80161240)
## 问：CSS 动画有哪些？


> - animation



animation、transition、transform、translate 这几个属性要搞清楚：

- animation：用于设置动画属性，他是一个简写的属性，包含6个属性
- transition：用于设置元素的样式过度，和animation有着类似的效果，但细节上有很大的不同
- transform：用于元素进行旋转、缩放、移动或倾斜，和设置样式的动画并没有什么关系
- translate：translate只是transform的一个属性值，即移动，除此之外还有 scale 等
### 参考资料


- [https://juejin.im/post/5b137e6e51882513ac201dfb#heading-2](https://juejin.im/post/5b137e6e51882513ac201dfb#heading-2)



## （3）问：用css2和css3分别写一下垂直居中和水平居中


### CSS2


水平居中：


- div + margin: auto;
- span + text-align



垂直居中


- 使用 position 然后 left/top 和 margin 的方式垂直居中（已知宽高和未知宽高）
- 使用 position + margin
- 使用 display: table-cell;



#### 已知宽高，进行水平垂直居中
```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .outer {
        position: relative;
        width: 400px;
        height: 600px;
        background: blue;
      }

      .inner {
        position: absolute;
        width: 200px;
        height: 300px;
        background: red;
        left: 50%;
        top: 50%;
        margin: -150px 0 0 -100px;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner"></div>
    </div>
  </body>
</html>

```
#### 宽高未知，比如 内联元素，进行水平垂直居中
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .outer {
        width: 400px;
        height: 600px;
        /* background: blue; */
        border: 1px solid red;
        background-color: transparent;
        position: relative;
      }

      .inner {
        position: absolute;
        background: red;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <span class="inner">我想居中显示</span>
    </div>
  </body>
</html>
```
#### 绝对定位的 div 水平垂直居中
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .outer {
        width: 400px;
        height: 600px;
        /* background: blue; */
        border: 1px solid red;
        background-color: transparent;
        position: relative;
      }

      .inner {
        position: absolute;
        background: red;
        left: 0;
        right: 0;
        bottom: 0;
        top: 0;
        width: 200px;
        height: 300px;
        margin: auto;
        border: 1px solid blue;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner">我想居中显示</div>
    </div>
  </body>
</html>
```
#### 图片和其他元素使用 display: table-cell; 进行垂直居中
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .outer {
        width: 400px;
        height: 600px;
        /* background: blue; */
        border: 1px solid red;
        background-color: transparent;
        display: table-cell;
        vertical-align: middle;
      }

      .inner {
        background: red;
        width: 200px;
        height: 300px;
        border: 1px solid blue;
        margin: 0 auto;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner">我想居中显示</div>
    </div>
  </body>
</html>
```
### CSS3
#### 垂直、水平居中
```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .outer {
        width: 400px;
        height: 600px;

        display: flex;
        
        /* 垂直居中 */
        align-items: center;
        
        /* 水平居中 */
        justify-content: center;
        border: 1px solid red;
        background-color: transparent;
      }

      .inner {
        background: red;
        width: 200px;
        height: 300px;
        border: 1px solid blue;
      }
    </style>
  </head>
  <body>
    <div class="outer">
      <div class="inner">我想居中显示</div>
    </div>
  </body>
</html>

```


## （2）问：visibility 和 display 的差别（还有opacity)


- visibility 设置 hidden 会隐藏元素，但是其位置还存在与页面文档流中，不会被删除，所以会触发浏览器渲染引擎的重绘
- display 设置了 none 属性会隐藏元素，且其位置也不会被保留下来，所以会触发浏览器渲染引擎的回流和重绘。
- opacity 会将元素设置为透明，但是其位置也在页面文档流中，不会被删除，所以会触发浏览器渲染引擎的重绘




## 问：opacity 可以有过渡效果嘛？


>  可以设置过渡效果



## 问：BFC 与 IFC 区别


BFC 是块级格式上下文，IFC 是行内格式上下文：


- 内部的 Box 会水平放置
- 水平的间距由 margin，padding，border 决定



### 参考链接：


- [https://juejin.im/entry/5938daf7a0bb9f006b2295db](https://juejin.im/entry/5938daf7a0bb9f006b2295db)
- [https://zhuanlan.zhihu.com/p/74817089](https://zhuanlan.zhihu.com/p/74817089)



## 问：BFC会与float元素相互覆盖吗？为什么？举例说明


不会，因为 BFC 是页面中一个独立的隔离容器，其内部的元素不会与外部的元素相互影响，比如两个 div，上面的 div 设置了 float，那么如果下面的元素不是 BFC，也没有设置 float，会形成对上面的元素进行包裹内容的情况，如果设置了下面元素为 overflow：hidden；属性那么就能够实现经典的两列布局，左边内容固定宽度，右边因为是 BFC 所以会进行自适应。


### 参考链接


- [https://zhuanlan.zhihu.com/p/25321647](https://zhuanlan.zhihu.com/p/25321647)



## 问：了解box-sizing吗？


box-sizing 属性可以被用来调整这些表现:

- `content-box`  是默认值。如果你设置一个元素的宽为100px，那么这个元素的内容区会有100px 宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。
- `border-box` 告诉浏览器：你想要设置的边框和内边距的值是包含在width内的。也就是说，如果你将一个元素的width设为100px，那么这100px会包含它的border和padding，内容区的实际宽度是width减去(border + padding)的值。大多数情况下，这使得我们更容易地设定一个元素的宽高。
## （2）什么是 BFC


   BFC（Block Formatting Context）格式化上下文，是 Web 页面中盒模型布局的 CSS 渲染模式，指一个独立的渲染区域或者说是一个隔离的独立容器。


### 形成 BFC 的条件


五种：

- 浮动元素，float 除 none 以外的值
- 定位元素，position（absolute，fixed）
- display 为以下其中之一的值 inline-block，table-cell，table-caption
- overflow 除了 visible 以外的值（hidden，auto，scroll）
- HTML 就是一个 BFC



BFC 的特性：

- 内部的 Box 会在垂直方向上一个接一个的放置。
- 垂直方向上的距离由 margin 决定
- bfc 的区域不会与 float 的元素区域重叠。
- 计算 bfc 的高度时，浮动元素也参与计算
- bfc 就是页面上的一个独立容器，容器里面的子元素不会影响外面元素。



## （2）问：了解盒模型吗？


CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：`外边距（margin）`、`边框（border）`、`内边距（padding）`、`实际内容（content）`四个属性。
CSS盒模型：**标准模型 + IE模型**


标准盒子模型：宽度=内容的宽度（content）+ border + padding


低版本IE盒子模型：宽度=内容宽度（content+border+padding），如何设置成 IE 盒子模型:
```css
box-sizing: border-box;
```


### 参考链接


- [https://zhuanlan.zhihu.com/p/74817089](https://zhuanlan.zhihu.com/p/74817089)



## 问：说一下你知道的position属性，都有啥特点？


static：无特殊定位，对象遵循正常文档流。top，right，bottom，left等属性不会被应用。
 relative：对象遵循正常文档流，但将依据top，right，bottom，left等属性在正常文档流中偏移位置。而其层叠通过z-index属性定义。
 absolute：对象脱离正常文档流，使用top，right，bottom，left等属性进行绝对定位。而其层叠通过z-index属性定义。
 fixed：对象脱离正常文档流，使用top，right，bottom，left等属性以窗口为参考点进行定位，当出现滚动条时，对象不会随着滚动。而其层叠通过z-index属性定义。
sticky：具体是类似 relative 和 fixed，在 viewport 视口滚动到阈值之前应用 relative，滚动到阈值之后应用 fixed 布局，由 top 决定。


## 问：两个div上下排列，都设margin，有什么现象？


- 都正取大
- 一正一负相加



问：为什么会有这种现象？你能解释一下吗


是由块级格式上下文决定的，BFC，元素在 BFC 中会进行上下排列，然后垂直距离由 margin 决定，并且会发生重叠，具体表现为同正取最大的，同负取绝对值最大的，一正一负，相加


BFC 是页面中一个独立的隔离容器，内部的子元素不会影响到外部的元素。


## 问：清除浮动有哪些方法？


不清楚浮动会发生高度塌陷：浮动元素父元素高度自适应（父元素不写高度时，子元素写了浮动后，父元素会发生高度塌陷）

- clear清除浮动（添加空div法）在浮动元素下方添加空div,并给该元素写css样式：{clear:both;height:0;overflow:hidden;}
- 给浮动元素父级设置高度
- 父级同时浮动（需要给父级同级元素添加浮动）
- 父级设置成inline-block，其margin: 0 auto居中方式失效
- 给父级添加overflow:hidden 清除浮动方法
- 万能清除法 after伪类 清浮动（现在主流方法，推荐使用）
```css
.float_div:after{
  content:".";
  clear:both;
  display:block;
  height:0;
  overflow:hidden;
  visibility:hidden;
}
.float_div{
  zoom:1
}
```




### 参考链接


- [https://juejin.im/post/5cc59e41e51d456e62545b66](https://juejin.im/post/5cc59e41e51d456e62545b66)
- [https://segmentfault.com/a/1190000013325778](https://segmentfault.com/a/1190000013325778)
- [https://juejin.im/post/5ca80d366fb9a05e3345dccf#heading-16](https://juejin.im/post/5ca80d366fb9a05e3345dccf#heading-16)
- [https://juejin.im/post/5e8d5268f265da480f0f9c6e#heading-33](https://juejin.im/post/5e8d5268f265da480f0f9c6e#heading-33)
- [https://juejin.im/post/5cc59e41e51d456e62545b66#heading-75](https://juejin.im/post/5cc59e41e51d456e62545b66#heading-75)
- [https://juejin.im/post/5a0c184c51882531926e4294#heading-50](https://juejin.im/post/5a0c184c51882531926e4294#heading-50)
- [https://juejin.im/post/5ce607a7e51d454f6f16eb3d#heading-34](https://juejin.im/post/5ce607a7e51d454f6f16eb3d#heading-34)
- [https://juejin.im/post/5e8b163ff265da47ee3f54a6#heading-12](https://juejin.im/post/5e8b163ff265da47ee3f54a6#heading-12)



