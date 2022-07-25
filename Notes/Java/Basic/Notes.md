## 注释

```java
// 单行注释

/* 多行注释 */

/**
 * JavaDoc
 */
```

### javaDoc

常用参数信息

- author:作者名
- version:版本号
- since:指明需要最早使用的jdk版本
- param:参数名
- return:返回值情况
- throws:异常抛出问题

生成javaDoc，在命令行中输入

```bash
javadoc -encoding UTF-8 -charset UTF-8 javaDocTest.java
```