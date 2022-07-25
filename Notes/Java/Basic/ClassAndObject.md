## 对象和类

### 类的定义

```java
public class TestClass {
	// 成员变量
	int id;
	// 类变量
	static String name;
	// 构造方法
	public TestClass() {
	}
	// 带参构造方法
	public TestClass(int id) {
		this.id = id;
	}
	// 成员方法
	public void Print() {
		System.out.println(id);
	}
}
```

### 创建对象

1. 声明：声明一个对象，包括对象名称和对象类型。
2. 实例化：利用关键字new来创建一个对象。
3. 初始化：使用new创建对象时，会调用构造方法初始化对象。

```java
TestClass obj1 = new TestClass();
TestClass obj2 = new TestClass(1);
```

### 访问变量和方法

```java
/* 实例化对象 */ 
Object referenceVariable = new Constructor();
/* 访问类中的变量 */ 
referenceVariable.variableName;
/* 访问类中的方法 */ 
referenceVariable.methodName();
```

## Java包

表示java文件的存储路径。

```java
package com.company.project.module; // 声明存储位置
```

### import语句

在java文件中，如果要调用其他java文件中定义的类/接口，就需要进行导入

```java
import java.io.*; // 导入java.io包下的所有内容
```