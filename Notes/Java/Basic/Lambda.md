## lambda表达式

函数式接口(Functional Interface)：任何接口，如果只包含唯一一个抽象方法，那么它就是一个函数式接口。

### 推导lambda表达式

```java
public class Testlambda {  
    // 3. 静态内部类  
    static class Like2 implements ILike {  
        @Override  
        public void lambda() {  
            System.out.println("lambda like 2");  
        }  
    }  
  
    public static void main(String[] args) {  
        ILike like = new Like1();  
        like.lambda();  
  
        like = new Like2();  
        like.lambda();  
  
        // 4. 局部内部类  
        class Like3 implements ILike  
        {  
            @Override  
            public void lambda() {  
                System.out.println("lambda like 3");  
            }  
        }  
        like = new Like3();  
        like.lambda();  
  
        // 5. 匿名内部类  
        like = new ILike() {  
            @Override  
            public void lambda() {  
                System.out.println("lambda like 4");  
            }  
        };  
        like.lambda();
    }  
}  
  
// 1. 定义一个函数式接口  
interface ILike {  
    void lambda();  
}  
  
// 2. 实现类  
class Like1 implements ILike {  
    @Override  
    public void lambda() {  
        System.out.println("lambda like 1");  
    }  
}
```

输出为

```
lambda like 1
lambda like 2
lambda like 3
lambda like 4
```

### 用Lambda表达式简化

```java
// 接在之前的程序后
like = () -> {  
    System.out.println("lambda like 5");  
};  
like.lambda();
```

### 带参数的lambda表达式

```java
public class TestLambdaParam {  
    public static void main(String[] args) {  
        ILove love = (int num) -> {  
            System.out.println("lambda love " + num);  
        };  
        love.lambda(0);  
  
        /* 1. 简化参数类型 */        // 当有多个参数时，所有参数都需简化或所有参数都不简化  
        love = (num) -> {  
            System.out.println("lambda love " + num);  
        };  
        love.lambda(1);  
  
        /* 2. 简化括号 */        // 当有多个参数时，括号不可省略  
        love = num -> {  
            System.out.println("lambda love " + num);  
        };  
        love.lambda(2);  
  
        /* 3. 简化大括号 */        // 当代码块中只有一行时，可简化大括号  
        love = num -> System.out.println("lambda love " + num);  
        love.lambda(3);  
    }  
}  
  
interface ILove {  
    void lambda(int num);  
}
```
