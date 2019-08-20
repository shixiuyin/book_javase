# day09_多态

## 一、回顾

``` xml
1.继承
	概念:子类继承父类的所有非私有属性和方法。
	优点:
		a.提高代码的重用性;
		b.简化代码操作
		c.更简单的完成子类操作 
	关键字:extends
	语法结构:[修饰符] class 类名 extends 父类名{}
	特点:
		a.java类属于单继承的，一个类只能有一个直接父类。一个父类可以派生出多个子类。
		b.Object是所有类的父类
	那些东西不能被子类继承:
		a.被private修饰的属性和方法 不能被继承
		b.不同包中被默认的修饰符修饰(缺省)的属性和方法，不能被继承
		c.构造方法不能被继承

2.方法的重写
	a.方法的名称，参数，返回值保持一致;
	b.重写的方法的访问修饰符不能比被重写的方法修饰符权限更低
	c.子类重写父类的方法(父类的父类)

3.super
	表示父类的对象
	a.可以调用父类的成员属性  super.属性
	b.可以调用父类的成员方法  super.方法()
	c.调用父类的构造方法

在所有的子类的构造方法中默认 调用父类无参的构造。也可以自己指定调用父类的有参构造。
super()调用父类的构造方法,不能和this()同时出现 。也只能在子类的构造方法中使用

对象的初始化过程:
	父类的属性---->父类的构造方法---->子类的属性------>子类的构造方法
```

## 二、多态

``` xml
1.概念
	同一个事物具有多种不同的表现形态。
2.特点
	a.在java中，子类对象可以替代父类对象使用(父类的引用指向子类的对象)Person p =  new Student();
	b.一个变量同一时刻 只能有一个确定的数据类型
	c.一个引用类型变量可能指向(引用)多种不同类型的对象
		Person p = new Student();  //List list =  new ArrayList();
		Object o = new Person();//Object类型的变量o，指向Person类型的对象
		o = new Student(); //Object类型的变量o，指向Student类型的对象
	d.一个引用类型变量如果声明为父类的类型，但实际引用的是子类对象，那么该变量就不能再访问子类中添加的属		 性和方法
	  编译的时候，看变量的类型。运行的时候看的是具体的对象。

3.方法的动态绑定
	在运行时候根据具体的对象，动态的绑定需要运行的方法。

4.多态的体现
	a.重载 :方法名一样，参数不同
	b.重写 :方法名和参数保持一致;
	c.父类的引用指向子类的对象(重写)
```



### 2.1 数据转换

``` xml
数据转换:必须存在关系才能转换。（继承） 
	    子类转父类---->自动类型转换  Person person = new Student();
		父类转子类---->强制转换  Student student = (Student)person();


instanceof:
	作用:用来判断对象是否是属于某一个类的对象  x instanceof A 
	注意:
		判断的对象和类存在子父类关系，否则编译出错
健壮性:

注意
	a.从子类到父类的类型转换可以自动进行
	b.从父类到子类的类型转换必须通过造型(强制类型转换)实现
	c.无继承关系的引用类型间的转换是非法的
	d.在造型前可以使用instanceof操作符测试一个对象的类型

```

``` java
package com.hzit01;

public class Test02 {
	/**
	 * 让所有的人进行自我介绍
	 * 
	 * @param person
	 */
	public void method(Object person) {
		// 一个父类 对应有多个子类 强转之前应该判断
		if (person instanceof Student) { // instanceof:判断某个对象 是否是属于某个类型 。作用：避免出现转为类型一次 Student--->Offices //非法
			Student student = (Student) person;
			student.getInfo();
			student.study();
		}
		if (person instanceof Offices) {
			Offices offices = (Offices) person;
			offices.getInfo();
			offices.work();
		}

		if (person instanceof Person) {//false
			Person p = (Person) person;
			p.getInfo();
		}
	}

	public static void main(String[] args) {
		Test02 test02 = new Test02();
		Person person = new Offices();
		test02.method("xxx");
	}
}

```



### 2.2 Object

``` xml
1.概述
	a.Object是所有类的父类（超类|基类）
	b.如果类没有使用extends继承自己的类，那么默认继续 Object
	c.所有的类都会自动享用Object中所有的属性和方法。

2.常用的方法(参照帮助文档API)
	a.hashCode()
		用户对象的hash码地址。就是一串数字  "A":65 "a":97
	b.toString()
		返回该对象的字符串表示;
		对象的组成:getClass().getName() + "@" + Integer.toHexString(hashCode());
		对象打印的时候会自动的调用toString()方法;
		重写toString():
			

	c.equals()
		i:Object对象中的方法，默认比较的也是内存地址;
		ii:特例:String,Date,File包装类(基本类型对应的包装类)比较的是内容是否相等，而不是地址
			   原因:默认重写了equals方法。
	
	d.== 全等于
		i:如果是基本类型的数据，比较的是内容
		ii:引用数据类型比较的就是内存地址是否相同


== 和 equals方法之间的区别:
	a.==可以比较基本类型也可以比较引用类型
	b.==和equals都是比较的引用地址，但是可以重写equals比较内容
	c.String,Date,File,包装类默认equals比较是内容，而不是地址
```

``` java
//重写toString
public class Person extends Object {

	String name = "张三";
	int age = 20;

	public void getInfo() {
		System.out.println("PersonInfo.....name:" + name + " age:" + age);
	}

	@Override
	public String toString() {
		return "Person [name:" + name + " age:" + age + " ]";
	}
    
    	// 重写equals方法
	@Override
	public boolean equals(Object obj) {
		if (this == obj) { // 表示地址一致，肯定是同一个对象
			return true;
		}
		if (obj instanceof Person) { // 传入的对象是相同的
			Person person = (Person) obj; // 传入对对象
			// 比较条件 名称和age都是相同才返回true
			if (this.name.equals(person.name) && this.age == person.age) {
				return true;
			}
			return false;
		} else { // 传入的对象和比较的对象 不是同一个类型 不用比
			return false;
		}
	}

}
 //测试
public static void main(String[] args) {
		Person person = new Person();
		System.out.println(person); //默认调用toString()
		System.out.println(person.toString()); // 调用就是子类重写父类的toString()
		
    	Person p1 = new Person("张三", 20);
		Person p2 = new Person("张三", 20);
		Person p3 = new Person("李四", 20);

		System.out.println(p1.equals(p2)); //true
		System.out.println(p1.equals(p3)); //false
	}

//Person [name:张三 age:20 ]
//Person [name:张三 age:20 ]
//true
//false
```



## 三、抽象类

``` xml
1.概念
	在面向对象的概念中，所有的对象都是通过类来描绘的，但是反过来，并不是所有的类都是用来描绘对象的，如果	  一个类中没有包含足够的信息来描绘一个具体的对象，这样的类就是抽象类。
	通俗:定义没有方法体的方法。员工只知道要工作，不知道具体的工作。

2.关键字
	abstract修饰

3.抽象类
	被abstrac修饰的类，称为抽象类。

4.抽象方法
	被abstrac修饰的方法，称为抽象方法。

5.特点
	a.抽象类可以包含抽象方法，也可以包含普通方法.
	b.抽象方法所在的类，必须是抽象类。抽象类并不一定需要抽象方法
	c.子类可以继承抽象类，子类必须实现抽象类中的抽象方法，或者将该子类也变成抽象类
```



**总结:**

``` xml
1.多态
	a.子类的对象可以代替父类的对象使用（父类的引用指向子类的对象）;
	b.同一时刻，一个变量只属于一个数据类型
	c.同一变量，可以指向不同的对象

	体现形式:
		方法的重写:同名同参
		方法的重载:同名不同参

2.数据类型的转换
	a.自动转换   从小到大
	b.强制转换   从大到小
	注意:引用类型强制转换的时候，必须存在父类(接口)关系。否则编译出错。
		可以使用instanceof 关键字进行判断;
		instanceof:判断某个对象是否是属于某个类型。 person instanceof Student

3.Object
	a.Object是所有类型的父类,超类，基类，根类
	b.没有使用 extends 关键字的时候，隐式的使用 extends Object
	c.Object主要的方法:
		hashCode(): 返回对象的hash码地址
		toString(): 返回对象的字符串格式  this.getClass.getName()+"@"+Integer.xxx(hashCode());
					重写toString()返回自己关注的信息
		equals():比较对象的引用地址是否相同。作用和 == 一样。
				return (this==obj);
				
				特例:String,Date,File包装类 他们的equals比较的就是具体的内容，而不是地址
					 因为这些重写了Object.equals()方法
				如果需要，自己也重写equals方法。

4.抽象类
	a.使用abstract修饰的类称为抽象类
	b.使用abstract修饰的方法称为抽象方法，抽象方法不能有方法体。

	注意:
		a.抽象类中可以有抽象方法，也可以有普通方法
		b.抽象类中可以没有抽象方法，都是由抽象的方法的类，一定是抽象类
		c.子类继承抽象的时候，必须实现所有的抽象方法，或者将该子类变成抽象类
```





## 四、接口

``` xml
1.概述
	接口是一个特殊的抽象类。接口中的所有方法都是抽象方法，所有的变量 都是静态常量；
	接口弥补了抽象类单一继承的不足；
	接口同样可以继承接口，并且可以实现多继承；

2.关键字
	定义接口:interface
	实现接口:implements
	语法:class  类 implements 接口名,接口名2{ //实现所有的抽象方法}

3.接口的特性
	a.接口不可以被实例化(必须有实现类)
	b.实现类必须实现接口中的所有方法。（接口中的方法都是抽象方法）
	c.实现类可以实现多个接口。接口与接口之间可以实现多继承
	d.接口中的所有变量都是静态常量
	c.接口看出一种能力，而不用关系具体是谁有这种能力。

4.接口和抽象类的区别
	a.接口是interface,抽象类 class。java类继承抽象类，实现接口
	b.接口是一个特殊的抽象类。接口中的所有方法都是抽象方法，抽象类中也可以拥有普通方法
	c.类单继承抽象类，类多实现接口。接口多继承接口
	d.接口中定义的属性必须是public，static，final类型的常量，而抽象类没有限制

5.接口的多态写法
	Paper paper = new A4();
	Paper paper = new B5();
```



## 五、关键字

``` xml
this:当前对象
super:当前的父类对象
final:最终的,不可改变的
static:静态的
```

### 5.1 final

``` xml
final可以修饰：

a.final修饰变量:常量
b.final修饰方法:不能被重写
c.final可以修饰类:不能被继承

常量:
	a.使用final修饰的变量，叫做常量
	b.常量在定义的时候必须赋值,一旦赋值不可更改
	c.常量的命名规则都是大写，多个单词使用_下划线   XXX_ZZZ_YYY

final修饰的方法，最终方法,该方法不能被重写:
	public final void sayHello() {
		System.out.println("hello，大家好我是:" + name);
	}

final修饰类，表示类不能被继承：
	public final class Person {
		private String name = "zhangsan";
		public final void sayHello() {
			System.out.println("hello，大家好我是:" + name);
		}
		public void method() {
		}
	}

String能不能被继承:
	不能，因为String类是被final修饰的。不能被其他类继承。
```



### 5.2 static

``` xml
静态的:
	静态变量:也可以使用类名.变量
	静态方法:	
		a.对象可以调用普通方法，也可以调用静态方法。
		b.静态方法属于类级别，所以可以不用对象，直接使用类调用  类名.方法()  |类名.属性()
	静态代码块：

静态的表示是属于类级别的。不需要对象可以直接使用。初始化过程在类加载的就已经完成。

```

``` java
public class Emp {

	String name = "张三";

	public Emp() {
		System.out.println("emp无参构造" + name);
	}

	// 代码块
	{
		System.out.println("普通代码块:" + name);
	}

	// 静态代码块
	static {
		System.out.println("静态代码块...");
	}

}

public class Son extends Emp {

	public Son() {
		System.out.println("son构造方法");
	}

	// 初始化功能
	{
		System.out.println("son代码块");
	}

	// 加载统一的配置文件
	static {
		System.out.println("son静态代码块");
	}

}

public class Test02 {
	public static void main(String[] args) {

		new Son();
	}
}

//静态代码块...
//son静态代码块
//普通代码块:张三
//emp无参构造张三
//son代码块
//son构造方法
```

接口中所有的变量都是静态常量：自动的添加public static final



``` xml
面向对象:
	作用:怎么去创建自己的对象。
高级部分:
	怎么去使用java提供好的对象。List,Map,IO,网络编程,多线程....
```





