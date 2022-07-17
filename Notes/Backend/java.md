# java

> 在已学习C++基础的条件下进行学习

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

## 数据类型

### 基本类型

- byte
- short
- int
- long
- float
- double
- char
- boolean

char数据类型，用UNICODE编码，表示范围为`'\u0000'` ~ `\uFFFF`（C语言中char所用的是ASCII编码)。

数据类型自动转化优先级: `byte,short,char -> int -> long -> float -> double`，强制转化方式与C语言相同。

> 在JDK7的新特性中，数字之间可用下划线进行分割，例如100000 = 100_000。

### 引用类型

- 类
- 接口
- 数组

## 变量

### 变量的作用域

- 类变量；在类中`static typeName variable;`
- 实例变量：在类中`typeName variable;`
- 局部变量：在方法中`typeName variable;`

### 常量

定义：`final typeName variable`

## 运算符

基本运算符：`+ - * / % -- ++`
幅值运算符：`=`
关系运算符：`> < >= <= == != instanceof`
逻辑运算符：`&& || !`
位运算符：`& | ^ ~ >> << >>>`
条件运算符：`?:`
扩展运算符：`+= -= *= /=`

## Java包机制

包的本质就是文件夹

包语句的语法格式为：`package pkg1[.pkg2[.pkg3...]];`

例如某文件的路径为`java/lang`，则该文件的包语句为`package java.lang;`

调用包的语法格式为：`import pkg1[.pkg2[.pkg3...]].(classname|*);`

## Scanner

> 工具包 java.util.Scanner 用于获取用户输入

```java
Scanner scanner = new Scanner(System.in);

if(scanner.hasNext()) // 检测用户是否有输入
{
    String str = scanner.next(); // 以空格或回车为输入结束符
}

if(scanner.hasNext()) // 检测用户是否有输入
{
    String str = scanner.nextLine(); // 以回车为输入结束符，读入一行数据
}

scanner.close();
```

进阶用法示例，判断输入是否为整数，如果是则输入，否则报错。
```java
Scanner scanner = new Scanner(System.in);
int x;

if(scanner.hasNextInt())
{
    x = scanner.nextInt();
    System.out.print("输入的整数为");
    System.out.println(x);
}
else
{
    System.out.print("输入不是整数");
}

scanner.close();
```

## 方法

> java只有值传递

- 基本数据类型使用值传递

- 传递对象传递的是引用的副本，而不是引用本身，所以没有引用传递，不叫引用传递

### 命令行传参

```java
public class CommandLine 
{
    public static void main(String args[]) 
    {
        for(int i = 0; i < args.length; i++)
        {
            System.out.println("args[" + i + "]: " + args[i]);
        }
    }
}
```

### 可变参数

> 一个方法中只能指定一个可变参数，它必须是方法的最后一个参数。

```java
public static void test(int ... number)
{
    for(int x : number)
    {
        System.out.println(x);
    }
}
```

## 数组

### 数组的定义

```java
// 长度为10的整形数组
int[] numbers = new int[10];
int numberss[] = new int[10]; // 同C++
// 二维数组
int[][] numbers2 = new int[10][10];
```

## Java面向对象

一个java文件中只能有一个public类，多个非public类

### 构造析构函数

```java
public class Person
{
    // 无参构造函数
    public Person()
    {
        
    }

    // 析构函数
    protected void finalize()
    {

    }
}
```

### 继承

- java中只有单继承，没有多继承
- 在java中，所有类默认继承object类
- private属性或方法继承后无法访问
- final类无法被继承

```java
public class Person
{

}

// Student类继承Person类
public class Student extends Person
{
    
}
```

> super

- 对父类的引用，类似于`this`
- `super()`方法为父类的构造函数，必须在子类构造函数的第一行调用
- 如果父类存在无参构造函数，子类构造函数中不需要显式调用父类的无参构造函数；如果父类不存在无参构造函数，则需要在子类的构造函数中显式调用父类的有参构造函数。

> 方法重写

`A a = new A();`在此处称左侧为引用类型，右侧为实际类型。

- 静态方法：方法的调用只与引用类型有关
- 动态方法：方法的调用与实际类型有关

```java
public class A
{
    public void print()
    {
        System.out.println("A.print");
    }
}

public class B extends A
{
    @Override // 注解：有一定功能的注释，可有可无
    public void print()
    {
        System.out.println("B.print");
    }
}
```

### 多态

- 父类的引用指向子类，与C++引用类型的多态相同。
- `x instanceof y`表达式：如果x与y无父子关系，则编译错误；若x的实际类型为y类的子类或为y类本身，则值为`true`，否则值为`false`。
- 类型强制转换：将父类引用类型强制转化为子类。

### static

规则与C++相同。

> 匿名代码块与静态代码块

```java
public class A
{
    // 匿名代码块，实例化对象时执行，执行顺序先于构造函数
    {

    }

    // 静态代码块，在类初始化时执行
    static
    {
        
    }
}
```

### 抽象类

被`abstract`关键字修饰的类。

- 抽象类不可被实例化
- 抽象类中可以有非抽象方法
- 抽象方法只能存在于抽象类中
- 继承抽象类的子类必须实现父类的所有方法，否则该子类也必须声明为抽象类
- 抽象类中可以有构造方法，是供子类创建对象时，初始化父类成员使用的

```java
public abstract class A
{
    public abstract void print()
    {

    }
}
```

### 接口

接口定义关键字：`interface`

- 接口中的方法默认由`abstract public`修饰
- 接口中的常量默认由`public static final`修饰
- 实现接口的类必须重写接口中的所有方法
- 接口可以实现多继承

```java
public interface A
{
    void printA();
}

public interface B
{
    void printB();
}

// 实现接口A, B
public class C implements A,B
{

}
```

### 内部类

- 内部类可以获得外部类的私有属性、方法

```java
public class Outer
{
    // 内部类
    public class Inner
    {

    }

    // 静态内部类
    public static class staticInner
    {

    }

    public void method()
    {
        // 局部内部类
        class PartInner
        {

        }
    }

    
}

class Main
{
    public static void main(String[] args)
    {
        Outer outer = new Outer();
        // 通过外部类实现内部类
        Outer.Inner inner = outer.new Inner();

        // 匿名类，不用将示例保存到变量中
        new Outer().method();

        // 匿名内部类
        new IClass()
        {

        };
    }
}

interface IClass
{

}
```

## 异常

### 捕获和抛出异常

- try
- catch
- finally
- throw
- throws

> 捕获异常

- 在idea中选中代码块按<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>t</kbd>

```java
try
{
    // try监控区域
}
catch(Exception e) // catch里的参数为想要捕获的异常类型
{
    // try检测到异常时会执行以下代码
    // Code
    e.printStackTrace();
}
catch(Error e) // 可以有多个catch捕获多种异常
{
    // try检测到异常时会执行以下代码
    // Code
    e.printStackTrace();
}
finally
{
    // 无论是否发生异常，都会执行
}
```

> 抛出异常

```java
public static void main(String[] args)
{
    try
    {
        
    }
    catch
    {

    }
}

// throws用来声明当前方法可能会出现某些异常，如果出现了异常，将由调用者来处理，声明了异常不一定会出现异常
public void test() throws ArithmeticException,ArrayIndexOutOfBoundsException
{
    // 主动抛出异常，一般在方法中使用
    // 如果在这个方法中处理不了这个异常，则需要在该方法中抛出异常
    throw new ArithmeticException();
}
```

### 自定义异常

1. 创建自定义异常类
2. 在方法中通过throw关键字抛出异常

```java
// MyException.java
public class MyException extends Exception{
    protected int detail;

    public MyException(int num)
    {
        super("MyException{" + num + "}");
        this.detail = num;
    }

    @Override
    public String toString()
    {
        return "MyException{" + detail + "}";
    }
}
```

``` java
// Hello.java
public class Hello {
    public static void main(String[] args) {
        try {
            test(19);
        } catch (MyException e){
            e.printStackTrace();
        }
    }

    public static void test(int num) throws MyException
    {
        if(num > 10)
        {
            throw new MyException(num);
        }
    }
}
```