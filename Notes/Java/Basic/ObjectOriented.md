## 面向对象

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