# css

> css规范
```css
selector{
    declaration
}
```

## css导入方式

**优先级满足就近原则**

1. 行内样式

```html
<h1 style="color: red"></h1>
```

2. style标签

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

3. 外部

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

