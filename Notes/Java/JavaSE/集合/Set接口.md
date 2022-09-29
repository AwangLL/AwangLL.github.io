# Set接口

Set接口也继承自Collection接口，它与Collection接口中的方法基本一致，并没有对Collection接口进行功能上的扩充。与List接口不同的是，Set接口中元素是无序的，并且都会以某种规则保证存入的元素不出现重复。

## HashSet

HashSet是Set接口的一个实现类，它所存储的元素是不可重复的。当向HashSet集合中添加一个元素时，首先会调用该元素的hashCode()方法来确定元素的存储位置，然后再调用元素对象的equals()方法来确保该位置没有重复元素。Set集合与List集合存取元素的方式都一样，但是Set集合中的元素是无序的。

HashSet存储元素的流程:

![](Pasted%20image%2020220922151132.png)

当向HashSet中存储自定义类对象时，需要重写`hashCode()`方法和`equals()`方法，否则HashSet中可能会出现重复的元素。

## LinkedHashSet

HashSet集合存储的元素是无序的，如果想让元素的存取顺序一致，可以使用Java提供的LinkedHashSet类，LinkedHashSet类是HashSet的子类，与LinkedList—样，它也使用双向链表来维护内部元素的关系。

## TreeSet

TreeSet是Set接口的另一个实现类，它内部采用平衡二叉树来存储元素，这样的结构可以保证TreeSet集合中没有重复的元素，并且可以对元素进行排序。所谓二叉树就是说每个节点最多有两个子节点的有序树，每个节点及其子节点组成的树称为子树，通常左侧的子节点称为“左子树”，右侧的节点称为“右子树”，其中左子树上的元素小于它的根结点，而右子树上的元素大于它的根结点。

TreeSet集合特有的方法:

|方法声明|功能描述|
|---|---|
|object first()|返回TreeSet集合的首个元素
|Object last()|返回TreeSet集合的最后一个元素
|Object lower(Object o)|返回TreeSet集合中小于给定元素的最大元素，如果没有返回null
|Object floor(Object o)|返回TreeSet集合中小于或等于给定元素的最大元素，如果没有返回null
|Object higher(Object o)|返回TreeSet集合中大于给定元素的最小元素，如果没有返回null
|Object ceiling(Object o)|返回TreeSet集合中大于或等于给定元素的最小元素，如果没有返回null
|Object pollFirst()|移除并返回集合的第一个元素
|Object pollLast()|移除并返回集合的最后一个元素

Java提供了两种TreeSet的排序规则，分别为自然排序和自定义排序。在默认情况下，TreeSet集合都是采用自然排序。

### 自然排序

自然排序要求向TreeSet集合中存储的元素所在类必须实现Comparable接口，并重写compareTo()方法，然后TreeSet集合就会对该类型元素使用compareTo()方法进行比较。compareTo()方法将当前对象与指定的对象进行顺序比较，返回值为一个整数，其中返回负整数、零或正整数分别表示当前对象小于、等于或大于指定对象，默认根据比较结果顺序排列。

```java
class Student implements Comparable {
	public int compareTo(Object obj) {
		Student stu = (Student)obj;
		if(this.age - stu.age > 0) {
			return 1;
		}
		if(this.age - stu.age == 0) {
			return this.name.compareTo(stu.name);
		}
		return -1;
		
	}
}
```

### 自定义排序

如果不想实现Comparable接口或者不想按照实现了Comparable接口的类中compareTo()方法的规则进行排序，可以通过自定义比较器的方式对TreeSet集合中的元素自定义排序规则。实现Comparator接口的类都是一个自定义比较器，可以在自定义比较器中的compare()方法中自定义排序规则。

```java
class Test {
	public static void main() {
		TreeSet set = new TreeSet(new Comparator<Student>() {
			@override
			public int compare(Student s1, Student s2) {
				int num = s1.getName().compareTo(s2.getName());
				return num==0 ? num : s1.getAge() - s2.getAge();
			}
		});
	}
}
```