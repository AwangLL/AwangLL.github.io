# List接口

List接口继承自Collection接口，List接口实例中允许存储重复的元素，所有的元素以线性方式进行存储。在程序中可以通过索引访问List接口实例中存储的元素。另外，List接口实例中存储的元素是有序的，即元素的存入顺序和取出顺序一致。

List接口继承了Collection接口的方法，也增加了一些方法，有如下常用方法。

|方法声明|功能描述|
|---|---|
|void add(int index,Object element)|将元素element插入List的index索引处
|boolean addAll(int index,Collection c)|将集合c所包含的所有元素插入到List集合的index索引处
|Object get(int index)|返回集合index索引处的元素
|Object remove(int index)|删除index索引处的元素
|Object set(int index, Object element)|将index索引处元素替换成element对象，并将替换后的元素返回
|int indexOf(Object o)|返回对象o在List中第一次出现的位置索引
|int lastIndexOf(Object o)|返回对象o在List中最后一次出现的位置索引
|List subList(int fromIndex, int toIndex)|返回从索引fromIndex (包括）到toIndex (不包括)处所有元素集合组成的子集合


# ArrayList 

是List接口的一个实现类，内部封装了一个可变长度的数组对象。

# LinkList

是List接口的一个实现类，内部维护了一个双向循环链表。

LinkList集合特有的方法:

|方法声明|功能描述|
|---|---|
|void add(int index,E element)|在集合的index索引处插入element元素
|void addFirst(Object o)|将指定元素o插入此集合的开头
|void addLast(Object o)|将指定元素o添加到此集合的结尾
|Object getFirst()|返回此集合的第一个元素
|Object getLast()|返回此集合的最后一个元素
|Object removeFirst()|移除并返回集合的第一个元素
|object removeLast()|移除并返回集合的最后一个元素
|boolean offer(Object o)|将指定元素o添加到集合的结尾
|boolean offerFirst(Object o)|将指定元素o添加到集合的开头
|boolean offerLast(Object o)|将指定元素o添加到集合的结尾
|Object peekFirst()|获取集合的第一个元素
|object peekLast()|获取集合的最后一个元素
|Object pollFirst()|移除并返回集合的第一个元素
|object pollLast()|移除并返回集合的最后一个元素
|void push(Object o)|将指定元素o添加到集合的开头

