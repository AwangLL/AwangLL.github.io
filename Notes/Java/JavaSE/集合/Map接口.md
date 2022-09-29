# Map接口

Map接口是一种双列集合，它的每个元素都包含一个键对象Key和值对象Value ,键和值对象之间存在一种对应关系，称为映射。Map中键对象Key不允许重复，访问Map集合中的元素时，只要指定了Key，就能找到对应的Value。

方法：

|方法声|功能描述|
|---|---|
|void put(Object key, object value)|将指定的值和键存入到集合中，并进行映射关联
|Object get(Object key)|返回指定键所映射的值;如果此映射不包含该键的映射关系，则返回null
|void clear()|移除所有的键值对元素
|V remove(Object key)|根据键删除对应的值，返回被删除的值
|int size()|返回集合中的键值对的个数
|boolean containsKey(Object key)|如果此映射包含指定键的映射关系，则返回true
|boolean containsValue(Object value)|如果此映射将一个或多个键映射到指定值，则返回true
|Set keySet()|返回此映射中包含的键的Set集合|
|collection\<V\> values() | 返回此映射中包含的值的Collection集合|
|Set<Map.Entry<K,V>>entrySet()|返回此映射中包含的映射关系的Set集合

## HashMap

HashMap集合是Map接口的一个实现类，HashMap集合中的大部分方法都是Map接口方法的实现。在开发中，通常把HashMap集合对象的引用赋值给Map接口变量，那么接口变量就可以调用类实现的接口方法。HashMap集合用于存储键值映射关系，但HashMap集合没有重复的键并且键值无序。

## 遍历Map

遍历Map集合中的所有的键，再根据键获取相应的值。

```java
Map map = new HashMap();

Set keySet = map.KeySet();
Iterator it = keySet.iterator();
while(it.hasNext()) {
	Object key = it.next;
	Object value = map.get(key);
	System.out.println(key + ":" + value);
}
```

先获取集合中所有的映射关系，然后从映射关系中取出键和值。

```java
Map map = new HashMap();

Set entrySet = map.entrySet();
Iterator it = entrySet.iterator();
while(it.hasMext()) {
	Map.Entry entry = (Map.Entry)(it.next());
	Object key = entry.getKey();
	Object value = entry.getValue;
	System.out.println(key + ":" + value);
}
```

Map还提供了一些操作集合的常用方法，例如，values()方法用于获取map实例中所有的value，返回值类型为Collection ; size()方法获取map集合的大小; containsKey()方法用于判断是否包含传入的键; containsValue()方法用于判断是否包含传入的值; remove()方法用于根据key移除map中的与该key对应的value等。

## LinkedHashMap

HashMap集合迭代出来元素的顺序和存入的顺序是不一致的。如果想让这Map集合中的元素迭代顺序与存入顺序一致，可以使用LinkedHashMap集合，LinkedHashMap是HashMap的子类，与LinkedList—样，LinkedHashMap集合
也使用双向链表维护内部元素的关系，使Map集合元素迭代顺序与存入顺序一致。

## TreeMap

HashMap集合存储的元素的键值是无序的和不可重复的，为了对集合中的元素的键值进行排序，Map接口还有了另一个可以对集合中元素键和值进行排序的实现类TreeMap。

## Properties

Map接口还有一个实现类Hashtable，它和HashMap十分相似，区别在于Hashtable是线程安全的。Hashtable存取元素时速度很慢，目前基本上被HashMap类所取代。但Hashtable类有一个很重要的子类Properties，应用非常广泛。Properties主要用于存储字符串类型的键和值，在实际开发中，经常使用Properties集合存储应用的配置项。

```java
// 假设要求以下配置: 
// Backgroup-color = red
// Font-size = 14px
// Language = chinese
Properties p = new Properties();
p.setProperty("Backgroup-color", "red");
p.setProperty("Font-size", "14px");
p.setProperty("Language", "chinese");
Enumeration names = p.propertyNames(); // 获取Enumeration对象所有键枚举
while(names.hasMoreElements()) {
	String key = (String)names.nextElement();
	String value = p.getProperty(key);
	System.out.println(key + "=" + value);
}
```

