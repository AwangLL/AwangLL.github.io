# HTML

> 声明为HTML文档

```html
<!DOCTYPE html>
```

> 声明编码方式为utf-8

```html
<head>
    <meta charset="utf-8">
</head>
```

> 注释

```html
<!-- description -->
```

## 标签

- ```<body></body>```成对的标签分别叫开放标签的闭合标签

单独出现的标签，如```<hr/>```称为自闭合标签

- 总标签```<html></html>```所有html内容需要写在总标签内
- 头部标签```<head></head>```
    - ```<title></title>```网页标题
    - ```<meta>```网页描述性标签，用来描述网页的一些信息
- 主体标签```<body></body>```

> 基本标签

- 标题标签```<h1></h1>```（1-6级标签）
- 段落标签```<p></p>```
- 换行标签```<br/>```
- 水平线标签```<hr/>```
- 字体样式标签
    - 粗体```<strong></strong>```
    - 斜体```<em></em>```
- 特殊符号
    - 空格```&nbsp;```
    - 大于号```&gt;```

> 图片标签

- ```<img src="path" alt="text"/>```

> 链接标签

- 地址链接```<a href="path"></a>```

- 锚链接
    - ```<a id=""></a>```标记
    - ```<a href="#id"></a>```跳转

- 功能链接
    - ```<a href="mailto:add@email.com></a>```

> 块元素和行内元素

- 块元素：独占一行(p、h1)
- 行内元素：可以排在一行(a、strong、em)

> 列表标签

- 有序列表

```html
<ol>
    <li></li>
    <li></li>
    <li></li>
</ol>
```

- 无序列表

```html
<ul>
    <li></li>
    <li></li>
    <li></li>
</ul>
```

- 自定义列表

```html
<dl> <!-- 标签 -->
    <dt></dt> <!-- 列表名称 -->
    <dd></dd> <!-- 内容 -->
</dl>
```

> 表格标签

```html
<table>
    <tr>
        <!-- colspan 跨列 -->
        <td colspan="2"></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
    </tr>
</table>
```

> 媒体标签

- 视频标签 ```<video src="path" control></vedio>```
- 音频标签 ```<audio src="path" control></vedio>```

> 页面结构分析

|标签|描述|
|-|-|
|header|头部|
|footer|脚部|
|section|主体|
|article|独立的文章内容|
|aside|相关内容或应用|
|nav|导航类辅助内容|

> iframe内联框架

```html
<iframe src=""></iframe>
```

> 表单

```html
<form action="" method="get">
    <p>username: <input type="text" name="username"> </p>
    <p>password: <input type="password" name="password"> </p>
    <p>
        <input type="submit">
        <input type="reset">
    </p>
</form>
```

- 文本框
    - ```<input type="text" name="">```
    - ```<input type="password" name="">```
- 单选框
    ```<input type="radio" value="" name="">```(```name```表示组)
- 多选框
    ```<input type="checkbox" value="" name="">```
- 按钮
    - 文本按钮```<input type="button" value="" name="">```
    - 图片按钮```<input type="image" src="">```
- 下拉框

```html
<select name="">
    <option value="1" selected></option>
    <option value="2"></option>
    <option value="3"></option>
    <option value="4"></option>
</select>
```

- 文本域
    ```<textarea name="" rows="" cols=""></textarea>```
- 文件域
    ```<input type="file" name="">```
- 邮箱
    ```<input type="email" name="">```
- URL
    ```<input type="url" name="">```
- 数字
    ```<input type="number" name="" max="" min="">```
- 滑块
    ```<input type="range" name="" max="" min="">```
- 搜索
    ```<input type="search" name="" max="" min="">```

> 表单属性

- 只读```readonly```

- 隐藏```hidden```

- 增强鼠标可用性

```html
<label for="mark">click</label>
<input type="text" id="mark">
```

- 禁用```disable```

> 表单初级验证

- ```placeholder```提示信息
- ```required```非空
- ```pattern```正则表达式