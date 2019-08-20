# day12_集合笔记

## 一、回顾

```xml
1.异常
	java运行过程中发生的不正常情况，称为异常;
2.异常处理的机制
	a.抛抓模型
3.异常的分类层次
	Throwable:所有异常和错误类的父类
		Exception:大部分异常都是由程序员代码不规范|问题造成
			RuntimeException:运行时异常,必须在运行的过程中，才会发生的异常
				 ArithmeticException:
				 ClassCastException:
				 IndexOutOfBoundsException:
				 NullPointerException:
			非运行时异常|强制异常:
			IOException:
			SQLException:
			
		Error:错误，程序员无法处理
4.try...catch...finally
	a.try:可能出现异常的代码块;
	b.catch:捕获try中出现的异常
		i:catch可以指定多个异常类型;
		ii:子类的异常类型，写在父类异常的catch块前面
		iii:exception是所以异常类的父类，所以它可以接受处理所有的异常类;
	c.finally:是否出现异常名，最终都会被执行的语句块
		finally是可以省略的,一般使用它来关闭资源，做一些善后的处理;

5.throws,throw
	throws:
		a.不使用try...catch捕获异常，直接将异常向上抛出;
		b.定义在方法名的后面   public void method() throws 异常类型{}
	throw:
		a.主动的抛出异常的类型(制造异常);
		b.在方法体的内部使用;

6.自定义异常
	根据需求定义符号自己要求的异常类;
	a.自定义异常类必须继承exception及其子类;
	b.定义有参数的构造方法，传递错误的显示信息(message)
```



## 二、java集合

``` xml
存储的介质：
	变量:单个类型单个可变的数据
	常量:单个类型单个不可变的数据
	数组:单个类型的多个数据,数组的长度不可变;
	集合:多个类型的多个数据,长度可变的

1.集合的概述
	a.Java 集合就像一种容器，可以把多个对象的引用放入容器中。
	b.Java 集合类可以用于存储数量不等的多个对象。（默认可以存在任意类型的对象）
	c.Java 集合可分为 Set、List 和 Map 三种体系
		Set:无序，不可重复
		List:有序，可重复
		Map:具有映射关系的集合(key-value)
	d.jdk5之前，所有的集合都可以存放Object类型的数据------>导致数组中具体的元素数据类型丢失(强转)
	  jdk5之后，引入了泛型，可以让集合记住自己是什么类型;---->List<Person>

2.java集合的架构图
	Collection:
          List:ArrayList,LinkedList
          Set:HashSet,TreeSet
	Map:
          HashMap
          TreeMap
          HashTable
```

![1553562896686](img\1553562896686.png)

## 三、Collection

``` xml
1.概述
	a.Collection 接口是 List、Set 和 Queue 接口的父接口
	b.该接口里定义的方法既可用于操作 Set 集合，也可用于操作 List 和 Queue 集合
2.Collection常用的方法
```

![1553564275546](img\1553564275546.png)

``` java
public static void main(String[] args) {

		// 创建了Collection的子类对象(使用其他也可以)
		Collection collection = new ArrayList<>();
		Collection collection2 = new ArrayList<>();
		// 1.添加元素 add
		collection.add("hello world");
		collection.add("hello java");
		collection.add(123);
		// collection.add(false);

		collection2.add("hello world");
		collection2.add(123);

		// collection.addAll(c); //集合添加集合

		// 移除
		// collection.clear();
		// 是否包含元素
		System.out.println("是否包含:" + collection.contains("hello javaxxx"));
		// collection.containsAll(c);//是否包含整个集合

		System.out.println("是否相等:" + collection.equals(collection2));
		System.out.println("==:" + (collection == collection2));

		System.out.println("hashCode:" + collection.hashCode());
		System.out.println("hashCode:" + collection2.hashCode());

		System.out.println("是否为空:" + collection.isEmpty());

		System.out.println("是否移除成功:" + collection.remove("hello worldxxx"));

		System.out.println("是否存在包含关系:" + collection.retainAll(collection2));
		System.out.println(collection);
		System.out.println(collection2);

		System.out.println("集合元素的大小:" + collection.size());

		// 转换为数组
		Object[] array = collection.toArray();
		for (Object object : array) {
			System.out.println(object);
		}

	}

```

### 3.1迭代器

``` xml
作用：就是用来获取集合或者数组的中的元素。类似for循环
创建方法:
	必须通过集合获取一个迭代器。
	Iterator iterator = collection.iterator();

对 collection 进行迭代的迭代器
常用的方法:
	hasNext():判断是否有元素可取
	next():使用前进行判断，获取当前的元素
```

![1553566341540](img\1553566341540.png)

``` java
public class Demo02 {

	public static void main(String[] args) {
		Collection collection = new ArrayList<>();
		// 1.添加元素 add
		collection.add("hello world");
		collection.add("hello java");
		collection.add(123);
		collection.add(123);

		// 2.遍历集合
		Iterator iterator = collection.iterator(); // 获取迭代器，用于迭代当前集合

		while (iterator.hasNext()) { // 判断是否还有元素可以读取
			Object next = iterator.next();
			System.out.println("对应的值:" + next);
			iterator.remove(); // 移除获取的元素
			System.out.println(collection);
		}

	}

}

```

**增强for底层就是迭代器**

``` java
public class Demo03 {

	public static void main(String[] args) {
		Collection collection = new ArrayList<>();
		// 1.添加元素 add
		collection.add("hello world");
		collection.add("hello java");
		collection.add(123);
		collection.add(123);

		/*
		 * 增强for循环
		 */
		for (Object obj : collection) {
			System.out.println(obj);
		}
	}
}

```



## 四、List

``` xml
1.概述
	a.一个元素有序、且可重复的集合，集合中的每个元素都有其对应的顺序索引
	b.List 允许使用重复元素，可以通过索引来访问指定位置的集合元素。
	c.List 默认按元素的添加顺序设置元素的索引。
	d.List 集合里添加了一些根据索引来操作集合元素的方法
	简单来说:List是支持有序，可重复的元素;

2.List接口新增的方法:
	void add(int index, Object ele)
	boolean addAll(int index, Collection eles)
	Object get(int index)
	int indexOf(Object obj)
	int lastIndexOf(Object obj)
	Object remove(int index)
	Object set(int index, Object ele)
	List subList(int fromIndex, int toIndex)
Collection中的所有方法，List也可以直接使用。因为Collection是List父接口;

3.List接口主要包含了三个实现类
	a.ArrayList 使用数组实现,线程不同步
	b.LinkedList 使用链表实现,线程不同步
	c.Vector 使用数组实现,线程同步
```

``` java
/**
 * List方法的使用
 * 
 * @author THINK
 *
 */
public class Demo04 {

	public static void main(String[] args) {
		// List在 Collection基础上添加了 操作index索引的方法
		List list = new ArrayList<>();

		list.add("a");
		list.add("b");
		list.add("1");
		list.add("3");
		list.add("c");
		list.add("2");
		list.add("c");
		System.out.println(list);

		// 给指定位置添加元素
		list.add(2, "java");
		System.out.println(list);

		// 获取指定位置的元素|类似数组
		Object object = list.get(0); // 下标从0开始
		System.out.println("下标对应的内容:" + object);

		// 根据元素获取下标的位置。如果不存在返回-1
		int indexOf = list.indexOf("c");
		int last = list.lastIndexOf("c");
		System.out.println("c所在的位置:" + indexOf);
		System.out.println("最后一次出现的位置:" + last);

		// 移除
		list.remove("c"); // 移除元素
		// list.remove("c");
		list.remove(1); // 如果是int类型，移除的就是集合的下标

		System.out.println(list);

		// 替换
		list.set(1, "hello world");
		System.out.println(list);

		// 截取子集合（包括前面，不包括后面）
		List subList = list.subList(1, 3);
		System.out.println(list);
		System.out.println(subList);
	}
}
```

### 4.1 ArrayList

``` xml
1.概述
	a.底层使用的是Object数组,拥有自己的下标;
	b.所有查询效率高(index)，添加和删除效率低(重写维护index,排序)。
	c.线程不同步，线程不安全。效率高;


2.length属性,length(),和size()分别在什么时候使用:
	length:数组获取长度使用length;
	length():String字符串的时候，使用
	size():集合的时候使用

		int[] arr = { 1, 2, 3, 4, 5, 6 };
		System.out.println("集合的长度:" + arrayList.size());
		System.out.println("字符串的长度:" + ("aaaa").length());
		System.out.println("数组的长度:" + arr.length);

```

![1553578969139](img\1553578969139.png)

![1553578994082](img\1553578994082.png)

``` java
// 通过 Arrays.asList创建固定长度的集合
public static void main(String[] args) {

		//返回固定长度的集合
		List<String> asList = Arrays.asList("zhangsan", "lisi", "wangwu");
		System.out.println(asList); 

	}
```



### 4.2 Vector

``` xml
1.概述
	a.是ArrayList的历史版本，使用方法一样;
	b.Vector属于线程安全的。执行效率慢
以后不会用。
```



### 4.3 LinkedList

``` xml
1.概述
	a.LinkedList是基于双向链表实现
	b.查询的效率较慢，但是插入和删除的效率高，只需要维护唯一前驱和后继就可以;不需要管理其他的元素
	c.线程不同步，线程不安全。效率高;

提供几个新的方法:
	xxxFirst()
	xxxLast()
```

``` java
LinkedList list = new LinkedList<>();

		list.add("a");
		list.add("b");
		list.add("c");
		list.add("d");

		list.addFirst("kk"); // 添加到最前面
		list.addFirst("yy"); // 添加到最前面

		list.addLast("zz"); // 添加到最后面
		list.addLast("xx");// 添加到最后面

		Object first = list.getFirst();
		System.out.println("第一个:" + first);
		Object last = list.getLast();
		System.out.println("最后一个:" + last);
		Object removeFirst = list.removeFirst();
		Object removeLast = list.removeLast();
		System.out.println("移除第一个:" + removeFirst);
		System.out.println("移除最后一个:" + removeLast);
```

### 4.4.泛型

``` xml
1.概念
	jdk1.5之前，Java 集合会丢失容器中所有对象的数据类型，把所有对象都当成 Object 类型处理;
	jdk1.5之后，引入泛型，作用让容器(集合)记住具体的数据类型
	泛型不能使用基本数据类型，基本数据类型得使用对应的包装类; int---Integer

2.语法
	使用"<泛型的类型>" 来指定具体的泛型类型   List<Integer> ====>该集合只能Integer类型的数据

3.作用
	a.让容器记住具体的数据类型;
	b.从容器集合中获取的数据，不需要进行强转(避免出现转换异常)
	c.非指定的该类型的数据，其余数据无法添加。
```

``` java
/**
 * 泛型
 * 
 * @author THINK
 *
 */
public class Demo01 {

	public static void main(String[] args) {
		List<Integer> arrayList = new ArrayList<>();
		arrayList.add(1);
		arrayList.add(2);
		arrayList.add(3);
		arrayList.add(4);
		// arrayList.add("a"); //非法 规定是Integer 传入的String

		// arrayList.size() ====>数组.length 获取长度

//		int[] arr = { 1, 2, 3, 4, 5, 6 };
//
//		System.out.println("集合的长度:" + arrayList.size());
//		System.out.println("字符串的长度:" + ("aaaa").length());
//		System.out.println("数组的长度:" + arr.length);

		int result = 0;
		// 统计集合中所有元素值得和
		for (int i = 0; i < arrayList.size(); i++) {
			int num = arrayList.get(i);
			result += num;
		}
		System.out.println("元素的和:" + result);

	}

}

```

### 4.5 **遍历集合的方式**

``` java
/**
	 * 普通for --通过下标
	 * 
	 * @param personList
	 */
	public static void method01(List<Person> personList) {
		for (int i = 0; i < personList.size(); i++) {
			Person person = personList.get(i);
			System.out.println(person);
		}
	}

	/**
	 * 增强for循环
	 * 
	 * @param personList
	 */
	public static void method02(List<Person> personList) {
		for (Person person : personList) {
			System.out.println(person);
		}
	}

	/**
	 * 迭代器
	 * 
	 * @param personList
	 */
	public static void method03(List<Person> personList) {
		// 1.得到迭代器
		Iterator<Person> iterator = personList.iterator();
		// 2.判断迭代器有没有值可以获取
		while (iterator.hasNext()) {
			// 3.获取
			Person person = iterator.next();
			System.out.println(person);
		}

	}

public static void main(String[] args) {

		// list存放的是引用地址
		List<Person> list = new ArrayList<>();

		Person zhangsan = new Person("zhangsan", 20);
		Person lisi = new Person("lisi", 20);
		Person wangwu = new Person("wangwu", 20);
		Person chenliu = new Person("chenliu", 20);

		list.add(zhangsan);
		list.add(lisi);
		list.add(wangwu);
		list.add(chenliu);
		// System.out.println(list);
		zhangsan.setAge(88);

		// 遍历List集合的三种方式
		// 1.普通循环
		// System.out.println(list);
		method03(list);

		// 2.增强for循环

		// 3.迭代器

	}
```



## 五、Set集合

``` xml
1.特点
	无序，不可重复;
2.概述
	Set 集合不允许包含相同的元素，如果试把两个相同的元素加入同一个 Set 集合中，则添加操作失败。
	Set 判断两个对象是否相同不是使用 == 运算符，而是根据 equals 方法

3.常用的方法
	Set父接口是Collection，所有Collection中的方法，Set也可以使用 
```

![1553591782297](img\1553591782297.png)



### 5.1 HashSet

``` xml
1.HashSet 具有以下特点：
	a.不能保证元素的排列顺序
	b.HashSet 不是线程安全的（异步）
	c.集合元素可以是null

2.概述
	a.HashSet 是 Set 接口的典型实现，大多数时候使用 Set 集合时都使用这个实现类。
	b.HashSet 按 Hash 算法来存储集合中的元素，因此具有很好的存取和查找性能。

3.Set常用的方法

需要让对象相同，重写hashCode和equals方法
4.hashCode()和equals()
	a.hashCode() 的作用是获取哈希码,它实际上是返回一个int整数,作用是确定该对象在哈希表中的索引位置
	b.hashCode()属于Object中的方法,所以所有的对象都拥有该方法;
	c.HashSet集合在每次add添加元素的时候，都会自定调用该元素的hashCode();
	d.只有当hashCode和equals都相同的都是，HashSet会把他们看成是同一个元素;(只保留一个)
	e.当各个元素的hashCode()返回的值相同的时候，才会去判断equals是否相同;
```

![1553592256779](img\1553592256779.png)

``` java
	@Override
	public int hashCode() {

//		System.out.println("hashcode.....");
//		final int prime = 3333; 
//		int result = 1;
//		result = prime * result + age;
//		result = prime * result + ((name == null) ? 0 : name.hashCode());

		// 目的:尽可能的避免不同的对象的hash冲突

		// name:zhangsan age:20 111+112 = 223
		// name:lisi age:21 110+113 = 223
		int result = age * 123 + name.hashCode();

		return result;
	}

	@Override
	public boolean equals(Object obj) {

		System.out.println("equlas....");
//		if (this == obj)
//			return true;
//		if (obj == null)
//			return false;
//		if (getClass() != obj.getClass())
//			return false;
//		Student other = (Student) obj;
//		if (age != other.age)
//			return false;
//		if (name == null) {
//			if (other.name != null)
//				return false;
//		} else if (!name.equals(other.name))
//			return false;

		if (obj instanceof Student) {
			Student stu = (Student) obj;
			if (this.name.equals(stu.name) && this.age == stu.age) {
				return true;
			}
		}

		return false;
	}
```

**Set集合遍历方式**

``` java
	//增强for
	public static void pirntln1(Set<Student> studentSet) {
		for (Student student : studentSet) {
			System.out.println(student.getName() + ":--->" + student.getAge());
		}
	}
	//迭代器
	public static void pirntln2(Set<Student> studentSet) {
		Iterator<Student> it = studentSet.iterator();
		while (it.hasNext()) {
			Student student = it.next();
			System.out.println(student.getName() + ":<----->" + student.getAge());
		}
	}
```





### 5.2 LinkedHashSet

``` xml
1.特点
	具有插入顺序,不可重复;

2.概述
	a.LinkedHashSet是HashSet的子类;
	b.Set 接口的哈希表和链表实现
	c.LinkedHashSet 性能插入性能略低于 HashSet

```

``` java
	public static void main(String[] args) {

		Set<Object> hashSet = new LinkedHashSet<>();
		hashSet.add("a");
		hashSet.add("b");
		hashSet.add("c");
		hashSet.add("d");
		hashSet.add("e");
		hashSet.add("a");
		hashSet.add(null);
		hashSet.add(1);

		System.out.println(hashSet);

	}
//执行结果：有序,不可重复
//[a, b, c, d, e, null, 1]
```





### 5.3 TreeSet

```xml
1.概述
	a.使用元素的自然顺序对元素进行排序;
	b.可以自定义排序
	c.此实现不是同步的，线程不安全;
	d.TreeSet不能存放 null
	c.不同存放多个没有任何关系的类型  Integer和String
```

``` java
public static void main(String[] args) {

		// 默认是自然排序
		Set<Object> hashSet = new TreeSet<>();
		hashSet.add("c");
		hashSet.add("d");
		hashSet.add("a");
		hashSet.add("b");
		hashSet.add("e");
		hashSet.add("a");

		// hashSet.add(null);
		// hashSet.add(1);

		System.out.println(hashSet);

	}
```

#### 1.TreeSet添加对象

``` xml
1.概述
	TreeSet中的对象都强制需要有可比性;
	java提供了Comparable接口强制进行排序;

2.Comparable用法
	a.自己的类实现Comparable接口;
	b.重写里面 compareTo()抽象方法。
		返回值:负整数、零或正整数，根据此对象是小于、等于还是大于指定对象。 

3.comparator用法(自定义比较器)
	a.创建comparator比较器
	b.创建TreeSet对象的时候，初始化传入比较器
		Set<Anmal> set =  new TreeSet<>(comparator)
	
```

``` java
// 1.按照age 进行排序
// 2.如果age相等，比较name

//实现Comparable接口
public class Person implements Comparable<Person> {

	private String name;
	private int age;

	//get/set
    //构造方法
    //toString

	/**
	 * 使用该方法进行排序
	 */
	@Override
	public int compareTo(Person o) {
		// 安装年龄进行排序
		int result = this.age - o.age;
		System.out.println("比较结果------->:" + result);
		// 如果年龄相同；看成是同一个对象
		if (result == 0) {
			result = this.name.compareTo(o.name); // String的自然排序 去比较
		}
		return result;
	}
}

public static void main(String[] args) {

		// 1.按照age 进行排序
		// 2.如果age相等，比较name
		Person zhangsan = new Person("b", 21);
		Person lisi = new Person("a", 21);
		Person zhangsan2 = new Person("zhangsan", 22);
		Person wangwu = new Person("wangwu", 19);
		Person zhaoliu = new Person("zhaoliu", 25);

		Set<Person> set = new TreeSet<>();
		set.add(zhaoliu);
		set.add(zhangsan);
		set.add(lisi);
		set.add(zhangsan2);
		set.add(wangwu);

		System.out.println(set);
	}

//结果
[Person [name=wangwu, age=19], Person [name=a, age=21], Person [name=b, age=21], Person [name=zhangsan, age=22], Person [name=zhaoliu, age=25]]

```



#### 2.自定义比较器

``` java

/**
 * 创建比较器
 * @author THINK
 *
 */
public class MyComparator implements Comparator<Animal> {

	@Override
	public int compare(Animal o1, Animal o2) {

		// 比较年龄
		int result = o1.getAge() - o2.getAge();
		if (result == 0) {
			result = o1.getName().compareTo(o2.getName());
		}

		return result;
	}

}
//使用
public class Demo06 {
	public static void main(String[] args) {

		// Collections.sort(list, c);
		// 创建自定义比较器
		MyComparator comparator = new MyComparator();
		Set<Animal> treeSet = new TreeSet<>(comparator);//构造对象，指定比较器

		Animal tiger = new Animal("m", 5);
		Animal dog = new Animal("dog", 8);
		Animal cat = new Animal("cat", 9);
		Animal pig = new Animal("a", 5);
		treeSet.add(dog);
		treeSet.add(cat);
		treeSet.add(pig);
		treeSet.add(tiger);
		System.out.println(treeSet);

		// List也可以使用
		System.out.println("---------------------------------------------------------------");

		List<Animal> arrayList = new ArrayList<>();
		arrayList.add(dog);
		arrayList.add(cat);
		arrayList.add(pig);
		arrayList.add(tiger);
		System.out.println(arrayList);

		// 指定比较器
		Collections.sort(arrayList, comparator);

		System.out.println("排序之后:" + arrayList);

	}
}

```







