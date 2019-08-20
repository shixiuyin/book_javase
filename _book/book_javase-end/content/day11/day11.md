# day11_java异常处理 

## 一、主要内容

``` xml
1.异常的概述
2.异常的处理机制
3.java中异常的分类层次,常见的异常
4.使用try....catch...finally捕获异常
5.使用throws 抛出异常
6.使用throw 制造异常
7.创建自定义的异常类型
```



## 二、异常的概述

``` xml
1.概述
	a.任何一种程序设计语言设计的程序在运行时都有可能出现错误
	b.捕获错误最理想的是在编译期间，但有的错误只有在运行时才会发生。
	c.对于这些错误，一般有两种解决方法：
		i:遇到错误就终止程序的运行。
		ii:由程序员在编写程序时，就考虑到错误的检测、错误消息的提示，以及错误的处理。

	d.在Java语言中，将程序执行中发生的不正常情况称为“异常”

2.java异常的分类
	a.Error:(错误)JVM系统内部错误、资源耗尽等严重情况。该异常程序员无法解决。
	b.Exception:(异常)其它因编程错误或偶然的外在因素导致的一般性问题。一般都是编码问题
			空指针访问
			试图读取不存在的文件
			网络连接中断
			下标越界
			除数为0....


3.层次结构
	|-Throwable:类是 Java 语言中所有错误或异常的超类
		|-error:错误
		|-exception:异常。是所有异常类的父类，超类;
			|-ClassNotFoundException: 没有找到类
			|-IOException:IO流异常类
			|-SQLException:SQL语句相关的异常
			|-RuntimeException:运行时异常
				|-ArithmeticException:除数为0异常
				|-ClassCastException:类型转换异
				|-NullPointerException:空指针异常
				|-IndexOutOfBoundsException:索引超出范围异常
					|-ArrayIndexOutOfBoundsException:数组下标越界
					|-StringIndexOutOfBoundsException:字符串下标越界
```

![1553477765579](img\1553477765579.png)

## 三、异常处理机制

``` xml
1.概述
	Java异常处理机制，将异常处理的程序代码集中在一起，与正常的程序代码分开，使得程序简洁，并易于维护。

2.处理模型
	Java提供的是异常处理的抛抓模型。
	a.Java程序的执行过程中如出现异常，会自动生成一个异常类对象，该异常对象将被提交给Java运行时系统，这		个过程称为抛出(throw)异常
	b.如果一个方法内抛出异常，该异常会被抛到调用方法中。如果异常没有在调用方法中处理，它继续被抛给这个调		用方法的调用者。这个过程将一直继续下去，直到异常被处理。这一过程称为捕获(catch)异常。
	c.如果一个异常回到main()方法，并且main()也不处理，则程序运行终止。
	d.程序员通常只能处理Exception，而对Error无能为力
```



## 四、try....catch...finally处理异常

``` xml
1.关键字
	a.try:将可能出现异常的代码放到try语句块中
	b.catch:如果try中真的出现异常，catch捕获该异常。搭配try一起使用，不能单独使用catch
	c.finally:最终的。表示不管是否发生异常，最终都会被执行的代码块。关闭资源

2.语法结构
	try
    {
 		......	//可能产生异常的代码
 	}
 	catch( ExceptionName1 e )
 	{
 		......	//当产生ExceptionName1型异常时的处置措施
 	}
 	catch( ExceptionName2 e )
 	{
 	...... 	//当产生ExceptionName2型异常时的处置措施
 	}  
 	[ finally{
 	......	 //无条件执行的语句
 			}  ]

3.注意
	a.一个try可以对应多个catch语句块
	b.catch需要制定具体的异常类型，如果没有匹配异常，就不会被捕获
	c.父类可以捕获所有的对应的子类异常类。Exception捕获所有的异常类型
	d.finally语句块 可以省略。如果写了不管是否发生异常，该代码都会被执行
	e.可以在catch语句块中，打印出异常的信息  e.printStackTrace()


问题:程序当中使用return，finally语句还会不会执行？如果会执行先执行谁？
	会执行,先试下finally在return。因为程序在return终止程序之前会检测有没有使用finally。

```

## 五、运行时异常和编译时候异常

``` xml
a.运行时异常
	程序在运行时候才会发生的异常。
	就算出处理异常，java也会自动处理。(打印错误信息，程序终止)

b.编译时异常|强制性异常
	该异常必须要经过处理，否则编译出错。
```

![1553484025051](img\1553484025051.png)

## 六、throws申明抛出异常

``` xml
异常处理方式:
	a.使用try...catch...finally自己捕获处理异常
	b.throws申明抛出异常
	
申明抛出异常:
	a.如果一个方法(中的语句执行时)可能生成某种异常，但是并不能确定如何处理这种异常，则此方法应显式地声明		抛出异常，表明该方法将不对这些异常进行处理，而由该方法的调用者负责处理。
	b.在方法声明中用 throws 子句可以声明抛出异常的列表，throws后面的异常类型可以是方法中产生的异常类		型，也可以是它的父类。

语法结构:
	[修饰符] 返回值 方法名(参数列表) throws 异常1,异常2,异常3...
	{
		// 方法体
	}

结论:将异常的处理权交给方法的调用者。调用者可以处理，也可以继续往上抛出....
	重写方法不能抛出比被重写方法范围更大的异常类型
```

``` java
public class Demo05 {

	public static int jisuan(int num1, int num2) {
		try {
			int result = num1 / num2;
			return result;
		} catch (Exception e) {
			System.out.println("除数不能为0");
			return 0;
		}

	}

	/**
	 * 不想处理异常 运行时候异常
	 * 
	 * @param num1
	 * @param num2
	 * @return
	 * @throws ArithmeticException :运行时候异常,编译时候可以不用处理
	 * @thwows SQLException:非运行时候异常,必须处理的异常 (测试)
	 */
	public int jisuan2(int num1, int num2) throws SQLException, RuntimeException, ArithmeticException {
		int result = num1 / num2;
		return result;

	}

	public static void main(String[] args) throws Exception {
		Demo05 demo05 = new Demo05();
		int jisuan;
		try {
			jisuan = demo05.jisuan2(30, 0);
			System.out.println("执行结果:" + jisuan);
		} catch (SQLException e) {
			System.out.println("sql异常");
			e.printStackTrace();
		} catch (ArithmeticException e) {
			System.out.println("除数不能为0");
			e.printStackTrace();
		}

		System.out.println("end...");
	}
}

```



## 七、throw人工抛出异常

``` xml
1.Java异常类对象除在程序执行过程中出现异常时由系统自动生成并抛出，也可根据需要人工创建并抛出

2.抛出的时候必须是Throwable|exception及其子类对象
		IOException e =new IOException();
		throw e;
	非异常类出现语法错误:
	    throw new String("want to throw");
	
throws和throw两者的区别:
	throws:
		a.定义在方法名称的后面;
		b.该方法不主动捕获异常，而是将方法向上抛出，让调用者去处理异常
	throw:
		a.定义在方法体的内部;
		b.用来主动生成一个新的异常类型;
	

```

## 八、自定义异常

``` xml
1.用户按照需求，自己定义的异常类

2.特点:
	a.必须继承异常类 Execption及其子类;
	b.定义一个String类型参数的构造方法，用来传递错误信息
```

``` java
/**
 * 定义异常类型
 * 
 * @author THINK
 *
 */
public class MyException extends Exception {

	private int idnumber;

	public MyException(String message, int idnumber) {
		super(message); // 传给父类信息  显示异常的信息
		this.idnumber = idnumber;
	}

	public int getIdnumber() {
		return idnumber;
	}

}

```







