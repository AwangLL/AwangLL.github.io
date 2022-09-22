# Iterator接口

Iterator主要用于迭代访问Collection中的元素，通常情况下Iterator对象也称为迭代器。

使用示例：

```java
ArrayList list = new ArrayList();
Iterator it = list.iterator();
while(it.hasNext()) {
	Object obj = it.next();
	System.out.println(obj);
}
```

> 并发修改异常: 在使用Iterator迭代器对集合中元素进行迭代时，如果调用了集合对象的remove()方法删除元素，之后继续使用迭代器遍历元素，会出现异常。

解决并发修改异常的策略:

1. 在删除元素后终止循环不再遍历
2. 使用迭代器本身的删除方法`it.remove()`，而不是list的删除方法

## foreach遍历

```java
for(Object obj : list) {
	System.out.println(obj);
}
```

> foreach循环不能修改数组中元素的值

