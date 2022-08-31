## 基本概念

### Web

- 静态web
	- html, css
	- 提供给所有人看的数据始终不会发生变化
- 动态web
	- 几乎所有的网站
	- 提供给所有人看的数据始终会发生变化
	- 技术栈：Servket/JSP，ASP，PHP

在Java中，动态web资源开发的技术统称为JavaWeb

### Web应用程序

可以提供浏览器访问的程序

- 能访问的所有资源都存放在某一台计算机上
- URL
- 统一的web资源会被放在同一个文件夹下，web应用程序-> Tomcat：服务器
- 一个web应用由多部分组成（静态web，动态web）
	- html、css、js
	- jsp、servlet
	- Java程序
	- jar包
	- 配置文件（Properties）

web应用程序编写完毕后，若想提供给外界访问：需要一个服务器来统一管理；

### 静态web

- `*.htm, *.html`，这些都是网页的后缀，如果服务器上一直存在这些东西，我们就可以直接进行读取。
- 