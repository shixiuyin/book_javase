# day18_ 反射笔记

## 一、回顾

``` xml
1.线程的同步
	同步:同一时刻只能发起一个请求;
	异步:同一时刻可以发起多个请求;
	线程的同步:在同一个时刻，只能有一个线程执行。
	作用:保护多个线程同时访问同一个资源，对资源造成破坏，数据混乱;
	在保证数据安全的情况下，尽可能的少的同步代码；

	同步的实现方式:
		隐式锁：synchronized
			每一个对象都有一个锁，自动的去上锁和解锁;
			多个线程使用同一个对象锁，才会有作用;
		显示锁：Lock
			手动上锁。手动解锁(推荐放在finally语句块里面)

2.对象锁
	a.每一个对象有且仅有一个锁;
	b.当一个线程获取该对象锁之后，其余线程在没有获取对象锁之前，无法调用该对象的同步方法;


3.线程的调度|通信
	wait:释放该对象锁，等待锁定池，不参与锁的竞争。直到被notify唤醒或者wait时间结束;
	notify:唤醒当前对象被wait 等待的任意一个线程;
	notifyall:唤醒当前对象被wait,唤醒所有的线程，但是对象锁只有一个,最终一个线程执行。其余现在就绪。

4.生产者和消费者 
	a.生产者和消费者线程
	b.多个线程操作的是同一个资源;
```



## 二、反射(Reflection)

``` xml
1.反射
	主要是指程序可以访问、检测和修改它本身状态或行为的一种能力
2.java反射
	是在运行状态中,一个类，都能够知道这个类的所有属性和方法;
	对于任意一个对象，都能够调用它的任意一个方法;
	这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制;

3.反射的作用
	通过已知的条件(String的类路径),获取该类中所有的信息;
	spring 中的xml配置文件，使用了反射。
	
	xml ===>解析xml(dom4j)
	<bean id="xxx" class="com.hzit.Dept">
		<prop name="dname" value="张三" />
	</bean>

4.Java反射机制主要提供了以下功能：
	在运行时构造任意一个类的对象
	在运行时获取任意一个类所具有的成员变量和方法(包括私有)
	在运行时调用任意一个对象的方法（属性）
	生成动态代理

```



### 2.1 反射API

``` xml
常用的包:
	java.lang.reflect;
常用的类:
	Class:表示反射的核心类
	Field:表示字段
	Method:表示方法
	Constructor:表示构造器
```



### 2.2 Class类

``` xml
1.概述
	对象反射后可以得到的信息：某个类的数据成员名、方法和构造器、某个类到底实现了哪些接口。对于每个类而	 言，JRE 都为其保留一个不变的 Class 类型的对象。一个 Class 对象包含了特定某个类的有关信息。

2.获取Class对象的三种方式
	a.类名.class属性;
	b.对象名.getClass()方法
	c.Class.forName(类的全路径(包名+类名)
3.常用方法
```

| 方法名                            | 功能说明                                                     |
| --------------------------------- | ------------------------------------------------------------ |
| static Class forName(String name) | 返回指定类名 name 的 Class 对象                              |
| Object newInstance()              | 调用缺省构造函数，返回该Class对象的一个实例                  |
| getName()                         | 返回此Class对象所表示的实体（类、接口、数组类、基本类型或void）名称 |
| Class getSuperClass()             | 返回当前Class对象的父类的Class对象                           |
| Class[]   getInterfaces()         | 获取当前Class对象的接口                                      |
| ClassLoader getClassLoader()      | 返回该类的类加载器                                           |

``` java
public static void main(String[] args) throws Exception {
		// com.hzit.bean.Person

		// 1.获取Class
		Class<?> clazz = Class.forName("com.hzit.bean.Student");

		// 2.根据Class newInstance获取默认构造器
		Object object = clazz.newInstance(); // 创建实例，调用无参构造
		System.out.println(object);

		// 3.获取名称
		String name = clazz.getName();
		System.out.println("类路径：" + name);

		// 4. 获取父类的对象
		Class<?> superclass = clazz.getSuperclass();
		System.out.println("父类的路径:" + superclass.getName());

		// 5.获取所有的接口
		Class<?>[] interfaces = clazz.getInterfaces();
		System.out.println(interfaces.length);
		for (Class<?> class1 : interfaces) {
			System.out.println("父接口的名称:" + class1.getName());
		}

		// 6.获取当前类的包名
		Package package1 = clazz.getPackage();
		System.out.println("包名:" + package1);
	}
```



#### 1.反射构造方法

``` xml
Constructor 提供关于类的单个构造方法的信息以及对它的访问权限
	newInstance(Object... initargs):调用构造方法

```



`**AccessibleObject**`

 是Constructor, Field, Method 的父类，提供了一个方法，取消java 面向对象的访问机制;

让修饰符失效。导致私有的信息，也可以被公开访问。

暴力破解:一般不推荐使用

`**setAccessible**(boolean flag)`   true:破解

``` java
public static void main(String[] args) throws Exception {
		Class<?> clazz = Class.forName("com.hzit.bean.Student");
		// Object newInstance = clazz.newInstance(); // 默认午餐构造

		// 获取 Constructor器对象 ，需要自己手动调用
		// 1.获取只有一个String类型的构造方法 (公共的构造)
//		Constructor<?> constructor = clazz.getConstructor(); // 获取只有一个String类型的构造方法
//		System.out.println(constructor);
//		constructor.newInstance(); // 调用指定的构造方法

		// 2.获取所有公共的构造方法
		// Constructor<?>[] constructors = clazz.getConstructors();

//		for (Constructor<?> constructor : constructors) {
//			Class<?>[] parameterTypes = constructor.getParameterTypes(); // 参数的类型
//			System.out.println("构造方法的名称:" + constructor);
//			for (Class<?> class2 : parameterTypes) {
//				System.out.println("\t对应的参数类型:" + class2.getSimpleName());
//			}
//
//			if (parameterTypes.length == 0) { // 无参构造
//				Object object = constructor.newInstance(); //调用无参
//			}
//
//		}

		// 3.获取指定类型的构造方法 包括公共和私有的
		// Constructor<?> declaredConstructor =
		// clazz.getDeclaredConstructor(String.class);

		// 4.获取所有的构造方法，包括私有的
		Constructor<?>[] declaredConstructors = clazz.getDeclaredConstructors();
		for (Constructor<?> constructor : declaredConstructors) {
			Class<?>[] parameterTypes = constructor.getParameterTypes(); // 参数的类型
			System.out.println("构造方法的名称:" + constructor);
			for (Class<?> class2 : parameterTypes) {
				System.out.println("\t对应的参数类型:" + class2.getSimpleName());
			}

			if (parameterTypes.length == 0) { // 无参构造
				// 私有的构造 无法访问
				// 暴力破解
				constructor.setAccessible(true);
				Object object = constructor.newInstance(); // 调用无参
			}
		}

	}
```



#### 2.反射属性

``` xml
1.Class中和Field字段相关的方法
	a.Field getField(String name) ;根据指定名称获取字段。（公共的字段，本类或者父类）
	b.Field[] getFields();获取所有字段。（公共的字段，本类或者父类）
	c.Field getDeclaredField(String name);根据指定名称获取字段(本类中的字段，包括私有的)
	d.Field[] getDeclaredFields();获取所有字段(本类中所有的字段，包括私有的)

2.Field字段信息
	Field 提供有关类或接口的单个字段的信息
常用的方法:
	get(Object obj) ：返回指定对象上此 Field 表示的字段的值
	getXXX(Object obj); 获取指定类型数据 getInt(Object obj) 
	setXXX(Object obj,xxx i);给该字段设置指定的值

```



#### 3.反射方法

``` xml
1.Class中和反射方法相关的方法
	a.Method getMethod(String name, Class<?>... parameterTypes): 参数1：方法名 参数2：参数类型
	b.Method[] getMethods():获取所有的方法(本类和父类中 公共的方法)
	c.Method getDeclaredMethod(String name, Class<?>... parameterTypes) :本类中指定方法(私有)
	d.Method[] getDeclaredMethods() 本类中所有方法(私有)

2.Method类中方法
	invoke(Object obj, Object... args):调用方法 参数二:实参列表
```









