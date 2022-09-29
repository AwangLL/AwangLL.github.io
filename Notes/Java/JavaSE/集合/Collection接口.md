# Collection 接口

Collection接口是Java单列集合中的根接口，它定义了各种具体单列集合的共性，其他单列集合大多直接或间接继承接口，Collection接口的定义如下所示：

```java
public interface Collection<E> extends Iterable<E> {
	
}
```

## Collection接口常用方法

|方法申明|功能描述|
|---|---|
|boolean add(Object o)|向集合中添加一个元素|
|boolean addAll(Collection c)|将指定集合c中的所有元素添加到本集合中|
|void clear()||
|boolean remove(Object o)|删除集合中的指定元素|
|boolean removeAll(Collection c)|删除集合中包含集合c中的所有元素|
|boolean isEmpty()|判断集合是否为空|
|boolean contains(Object o)|判断集合中是否包含某个元素|
|boolean containsAll(Collection c)|判断集合中是否包含指定集合c中的所有元素
|Iterator iterator()|返回集合的的迭代器(Iterator)，迭代器用于遍历该集合所有元素
|int size()|获取集合元素个数


