# css

### css规范

```css
selector{
    declaration
}
```

## css导入方式

**优先级满足就近原则**

### 行内样式

```html
<h1 style="color: red"></h1>
```

### style标签

```html
<head>
    <style>
        h1 {
            color:red;
        }
    </style>
</head>
<body>
    <h1></h1>
</body>
```

### 外部

```html
<head>
    <!-- 引用式 -->
    <link rel="stylesheet" href="css/style.css">
    <!-- 导入式 CSS2.1特有-->
    <style>
        @import url("css/style.css");
    </style>
</head>
```

## 选择器

作用：选择页面上的某一个或某一类元素。

### 基本选择器

优先级：id > class > 标签

- 标签选择器

标签选择器会选择页面上所有的这个标签

```html
<head>
    <style>
        h1 {
            color:red;
        }
        p{
            font-size:20px;
        }
    </style>
</head>
<body>
    <h1>head 1</h1>
    <p>paragraph</p>
</body>
```

- 类选择器

格式：.class的名称

可以复用

```html
<head>
    <style>
        .hd1{
            color:red;
        }
        .hd2{
            color:blue;
        }
    </style>
</head>
<body>
    <h1 class="hd1">header 1</h1>
    <h1 class="hd2">header 2</h1>
</body>
```

- id选择器

格式：#id名称

id唯一，不可复用

```html
<head>
    <style>
        #hd1{
            color:red;
        }
        #hd2{
            color:blue;
        }
    </style>
</head>
<body>
    <h1 id="hd1">header 1</h1>
    <h1 id="hd2">header 2</h1>
</body>
```

### 层次选择器

- 后代选择器

在某个元素的后面

```css
body p {
    background-color: red;
}
```

- 子选择器

只有一级

```css
body > p {
    background-color: red;
}
```

- 相邻兄弟选择器

同级同类 向下相邻

```css
.classp + p {
    background-color: red;
}
```

- 通用选择器

当前选中元素向下的所有兄弟元素

```css
.classp ~ p {
    background-color: red;
}

```

### 结构伪类选择器

```css
/* 无序列表的第一个子元素 */
ul li:first-child {
    background-color: red;
}
```

### 属性选择器

中括号中属性名 = 属性值（正则）

`=` 绝对等于
`*=` 包含
`^=` 以...开头
`$=` 以...结尾

```css
/* 选中a标签中带有id属性的 */
a[id] {
    background: black;
}
/* 选中a标签中id属性为first的 */
a[id=first] {
    background: red;
}
```

## 美化网页

`span`标签: 重点要突出的字，用`span`标签套起来

### 字体样式

|标签|含义|
|---|----|
|font-family|字体|
|font-size|字体大小|
|font-weight|字体粗细|
|color|字体颜色|
|font|字体样式|

font用法: <字体风格> <字体大小> <字体>

### 文本样式

|标签|含义|
|---|----|
|text-align|文本对齐方式|
|text-indent|首行缩进|
|height|文本高度|
|line-height|行高(单行文字上下居中)|
|text-decoration|文本装饰|
|vertical-align|水平对齐|
|text-shadow|文本阴影|

### 超链接伪类

```css
a{
    color:black
}
/* 鼠标悬停 */
a:hover{
    color:red
}
```

### 列表样式

```html
<!DOCTYPE html>
<head>
    <meta charset="utf-8">
    <title>列表样式</title>
    <style>
        #nav{
            width: 300px;
        }
        .title{
            font-size: 18px;
            font-weight: bold;
            text-indent: 1em;
            line-height: 35px;
            background-color: red;
        }
        ul{
            background-color: gray;
        }
        ul li{
            height: 30px;
            list-style: none;
            text-indent: 1em;
        }
        a{
            text-decoration: none;
            color: black;
        }
        a:hover{
            color: red;
        }
    </style>
</head>
<body>
<div id="nav">
    <h2 class="title">全部商品分类</h2>

    <ul>
        <li><a href="#">图书</a></li>
        <li><a href="#">电器</a></li>
    </ul>
</div>
</body>
```

### 背景图像

```html
<!DOCTYPE html>
<head>
    <meta charset="utf-8">
    <title>背景图片</title>
    <style>
        div{
            width: 1000px;
            height: 700px;
            border: 2px solid red;
            background-image: url("https://www.minecraft.net/content/dam/minecraft/misc-photos/China_Version.png");
            /* 默认为平铺 */
        }
        .div1{
            /* 水平平铺 */
            background-repeat: repeat-x;
        }
        .div2{
            /* 垂直平铺 */
            background-repeat: repeat-y;
        }
        .div3{
            /* 不平铺 */
            background-repeat: no-repeat;
        }
    </style>
</head>
<body>
<div class="div1"></div>
<div class="div2"></div>
<div class="div3"></div>
</body>
```

### background属性

```css
div{
    /* 背景颜色 背景图片 背景位置 平铺方式 */
    background: red url("") 10px 10px no-repeat;
}
```

> 渐变背景代码生成：[Grabient](https://www.grabient.com)


## 盒子模型

- margin: 外边距
- border: 边框
- padding: 内边距

### border

```css
div{
    /* 边框粗细 边框样式 边框颜色 */
    border: 2px solid red;
    /* 圆角边框 */
    border-radius: 0px;
}
```

### margin

> 可用于居中 `margin: 0 auto;`

```css
div{
    /* 上下左右 */
    margin: 0;
    /* 上下 左右 */
    margin: 0 0;
    /* 上 右 下 左 */
    margin: 0 0 0 0;
}
```

### padding

格式与margin相同

### 阴影

```css
div{
    box-shadow: 10px 10px 100px yellow;
}
```

## 块级元素和行内元素

块级元素:`h1-h6` `p` `div` `ul li`

行内元素:`span` `a` `img` `strong`

行内元素可以被包含在块级元素中，反之不可以

`display`属性:

- `block`: 块元素
- `inline`: 行内元素
- `inline-block`: 块元素但是可以在一行
- `none`: 隐藏

## 浮动

`float`属性:

- `left`: 左浮
- `right`: 右浮

`clear`属性的含义为某一侧不允许有浮动元素

- `left`:左侧不允许有浮动元素
- `right`:右侧不允许有浮动元素
- `both`:两侧都不允许有浮动元素
- `none`

### 父级边框塌陷问题

浮动会使得元素脱离父级元素边框的范围

- 增加父级元素高度
- 增加一个空`div`，空`div`的属性
    ```css
    div{
        clear: both;
        margin: 0;
        padding: 0;
    }
    ```
- 父级元素添加`overflow`属性，属性值为`hidden`
- 在父类添加一个伪类
    ```css
        #father:after{
            content: '';
            display: block;
            clear: both;
        }
    ```

## 定位

### 相对定位

相对于自己原来的位置进行偏移

`position: relative`

```css
#box{
    position: relative;
    top: 20px;
    right: 20px;
    bottom: 20px;
    left: 20px;
}
```

### 绝对定位

没有父级元素定位的情况下，相对于浏览器定位；父级元素存在定位的情况下（父级元素包含属性`position: relative;`，相对于父级元素定位。

`position: absolute`

```css
#box{
    position: absolute;
    top: 20px;
    right: 20px;
    bottom: 20px;
    left: 20px;
}
```

### 固定定位

`position: fixed`

```css
#box{
    position: fixed;
    top: 20px;
    right: 20px;
    bottom: 20px;
    left: 20px;
}
```

### z-index

`z-index`默认为0

属性设置方法: `z-index: 0`
