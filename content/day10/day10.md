# day10_常用的类和对象

``` xml
回顾:
	接口的使用:
		定义接口:interface
		实现接口:implements
		类可以多实现接口,接口可以多继承接口:
			class 类名 implements 接口名,接口名2{}
			interface 接口名 extends 父接口,父接口2{}
	接口和抽象类的区别:
		
	final: 最终的
		final修饰变量:常量
		final修饰方法:表示该方法不能被重写
		final修饰类:表示类不能被继承
	static:静态的
		属于类级别,可以直接使用类名.方法名() 类名.属性名   Person.getName(); Person.name;

面向对象：怎么创建对象，对象的语法结构
如何使用java，提供给我们的类

1.主要的内容
	a.API的使用
	b.包装类
	c.String
```



## 一、API的使用

``` xml
java官方提供的帮助文件。

java常用的五大包
	java.lang:语言包(java的默认包) String,Object,包装类
	java.io:操作IO流及文件相关的包 File FileInputStream...
	java.util:工具包  Arrays,Scanner,List...
	java.net:网络包 Socket,ServerSocket...
	java.sql:数据库相关的包 Connection,ResultSet,
	java.text:格式化操作 


查询帮助文档:
	a.查找类，两种方式；索引(索引),根据包的路径名

主要内容:

```

![1553241767848](img\1553241767848.png)

![1553242023728](img\1553242023728.png)

![1553242261598](img\1553242261598.png)



## 二、包装类的使用

```xml
包装类就是可以直接将简单类型的变量表示为一个类。
除了 int和char之外，所有的包装类就是将基本类型的首字母大写

JDK1.5之后实现自动的拆箱和封箱
装箱：从基本类型转换为包装类
拆箱:从包装类转换为基本类型

包装是引用类型的数据
```

![1553242581917](img\1553242581917.png)

包装类常用的方法:

![1553242845647](img\1553242845647.png)

## 三、String

``` xml
1.概念
	String 类代表字符串;使用""表示String 类型的值;
	字符串是常量；它们的值在创建之后不能更改;

2.String常用的构造方法
```

 

| String构造方法                                               | 简介                                           |
| ------------------------------------------------------------ | ---------------------------------------------- |
| String()                                                     | 创建一个新的字符串，但是值为空                 |
| String(byte [] bytes)                                        | 根据byte数据创建String,编码使用平台默认        |
| String(byte[] bytes,String charsetName);                     | 根据byte数据创建String，使用指定的编码方式     |
| String(byte[] bytes,  int offset, int length, String charsetName) | 根据byte数据创建String，规定长度和指定编码方式 |
| String(char [] ch)                                           | 根据字符数组创建对象,使用默认编码              |
| String(char [] ch,String charsetName)                        | 根据字符数组创建对象,使用指定的编码方式        |

``` java
public static void main(String[] args) throws Exception {
		// 将字符串转为数组(字符数组，字节数组)
		String str = "abcdefghigkl";

		byte[] bytes = str.getBytes();
		for (byte b : bytes) {
			System.out.println(b);
		}

		// 将字节数组 变为String
		String string = new String(bytes);
		System.out.println(string);
		String string2 = new String(bytes, "UTF-8");
		System.out.println(string2);

		String string3 = new String(bytes, 4, 5); // 指定需要截取的范围
		String string4 = new String(bytes, 2, 5, "UTF-8");
		System.out.println(string3);
		System.out.println(string4);

	}
```

``` java
3.常用的方法
	public static void main(String[] args) throws Exception {
		String str = "Hello World hello java我";
		System.out.println("获取字符串的长度:" + str.length());
		System.out.println("equals:" + str.equals("hello"));

		String lowerCase = str.toLowerCase();
		String upperCase = str.toUpperCase();
		System.out.println("转换为小写:" + lowerCase + "---->" + str);
		System.out.println("转换为大写:" + upperCase + "---->" + str);

		char ch = str.charAt(4);// 根据下标获取指定字符 从0开始计算
		System.out.println(ch);

		// 能找到返回值具体位置，否则返回-1.表示没有该字符
		int indexOf = str.indexOf("o"); // 指定字符 第一次出现的问题 从0开始计算
		System.out.println("第一次出的位置:" + indexOf);

		int indexOf2 = str.indexOf("o", 5); // 从指定位置找，下标还是从0开始计算
		System.out.println("从第五个位置开始计算:" + indexOf2);

		int lastIndexOf = str.lastIndexOf("o");
		System.out.println("最后一个出现的位置:" + lastIndexOf);

		int lastIndexOf2 = str.lastIndexOf("o", 5);
		System.out.println("最后一个出现的位置:" + lastIndexOf2);

		String substring = str.substring(1);
		System.out.println("截取:" + substring);

		String substring2 = str.substring(1, 3); // 包括前面，不包括后面的下标 1 2 顾前不顾尾
		System.out.println("截取:" + substring2);

		String[] split = str.split("o");
		for (String s : split) {
			System.out.println(s);
		}

		System.out.println("是否相等:" + ("a".compareTo("b")));

		System.out.println("是否包括:" + (str.contains("a")));

		System.out.println("是否是java结尾:" + (str.endsWith("java")));
		System.out.println("是否是java开始:" + (str.startsWith("java")));

		// 提供两个获取数组的方法 （字符串数组，字节数组）
		byte[] bytes = str.getBytes("utf-8");
		for (byte b : bytes) {
			System.out.println("字节:" + b);
		}
		char[] charArray = str.toCharArray();
		for (char c : charArray) {
			System.out.println("字符:" + c);
		}

		String string = new String(bytes, "utf-8");
		String string2 = new String(charArray);
		System.out.println(string);
		System.out.println(string2);
		
		System.out.println(str.replace("a", "AAAA"));//替换

	}
```



## 四、StringBuffer

``` xml
1.概述
StringBuffer线程安全的字符串缓冲区;
StringBuffer是一个内容可变的字符串;(String是内容不可变)

2.构造方法
	StringBuffer() :创建一个空的字符串缓冲区，初始容量为16个字符
	StringBuffer(String str):构造一个字符串缓冲区，并将其内容初始化为指定的字符串内容。

3.常用方法
	toString():转换为String
	charAt():
	indexOf():
	length():
	replace():
	subString():

	append():追加内容
	reverse():倒序，反转。将内容反转
	insert():插入
	delete(): 删除指定区域的数据
	deleteCharAt():删除指定位置的值
```

``` java
	public static void main(String[] args) {

		StringBuffer sbf = new StringBuffer();
		StringBuffer stringBuffer = new StringBuffer("hello");

		System.out.println(sbf.toString());
		System.out.println(stringBuffer.toString());

		// 和String一样的方法
		char charAt = stringBuffer.charAt(1);
		System.out.println(charAt);

		// 自己新增的方法
		stringBuffer.append("world").append("hello java"); // 追加字符串
		System.out.println(stringBuffer);

		stringBuffer.reverse(); // 反转 整个StringBuffer进行了反转

		System.out.println(stringBuffer);

		stringBuffer.insert(4, "xxxxxx"); //向指定位置添加值
		System.out.println(stringBuffer); 
        
        stringBuffer.delete(4, 10); // 删除指定区域的数据
		System.out.println(stringBuffer);

		stringBuffer.deleteCharAt(0); // 删除指定位置的值
		System.out.println(stringBuffer);
	}
```



## 五、StringBuilder 

``` xml
使用方法和StringBuffer一样。
StringBuffer是同步的，线程安全的。效率相对低
StringBuilder是线程不安全，效率高;

String,StringBuffer,StringBuilder之间的区别:
	a.String是内容不可变得,后者内容是可变的
	b.String,StringBuffer是线程安全的，StringBuilder是线程不安全的
```



## 六、Math

``` xml
和数学相关的对象

常用方法:
```

| 方法            | 说明                                      |
| --------------- | ----------------------------------------- |
| int abs()       | 返回输入参数的绝对值                      |
| double cbrt()   | 返回输入double参数值得立方根              |
| double ceil()   | 返回大于或等于参数的最小double值   \|取大 |
| double floor()  | 返回最大值double值   \|取小               |
| double max()    | 返回两个值，参数较大的一个                |
| double min()    | 返回两个值，参数较小的一个                |
| double random() | 返回一个0.0到1.0之间的随机数              |
| int round()     | 返回最接近输入数的int值                   |

``` xml
public static void main(String[] args) {

		System.out.println("绝对值:" + Math.abs(-0));
		System.out.println("立方根:" + Math.cbrt(4));

		System.out.println("向上取整:" + Math.ceil(10.01));
		System.out.println("向上取整:" + Math.ceil(10.5));
		System.out.println("向上取整:" + Math.ceil(10.9));

		System.out.println("向下取整:" + Math.floor(10.01));
		System.out.println("向下取整:" + Math.floor(10.5));
		System.out.println("向下取整:" + Math.floor(10.9));

		System.out.println("四舍五入:" + Math.round(10.01));
		System.out.println("四舍五入:" + Math.round(10.5));
		System.out.println("四舍五入:" + Math.round(10.9));

		System.out.println("最大:" + Math.max(20.02, 20.01));
		System.out.println("最小:" + Math.min(20.02, 20.01));
		

		// 随机生成 1-33
		int random = (int) (Math.random() * 33) + 1;
//		while (true) {
//			int random = (int) (Math.random() * 33) + 1;
//
//			System.out.println(random);
//			if (random <= 0 || random > 33) {
//				break;
//			}
//
//		}

//		while (true) {
//			double random = Math.random();
//			System.out.println("随机数(大于等于0小于1)" + random);
//			if (random == 0) {
//				break;
//			}
//		}

	}
```



## 七、Random 

``` xml
Random类提供了产生各种类型随机数的方法
Math中的random方法只能产生double类型的随机数
Random只有一个无参的构造方法，创建方式如下：Random rd = new Random();
```

| 方法                | 说明                                      |
| ------------------- | ----------------------------------------- |
| int nextInt()       | 产生一个整形随机数                        |
| int nextInt(int n)  | 产生一个0~n的随机整数值，包括0，但不包括n |
| double nextDouble() | 产生一个0.0~1.0的double型随机数           |
| float nextFloat()   | 产生一个0.0~1.0的float型随机数            |
| long nextLong()     | 产生一个long型随机数                      |

``` java
public static void main(String[] args) {
		Random random = new Random();

		System.out.println("随机整数:" + random.nextInt());
		System.out.println("随机整数:" + random.nextInt(33)); // 0-32 :不包括33 [0-32]
		System.out.println("double：" + random.nextDouble());
		System.out.println("boolean:" + random.nextBoolean());

//		while (true) {
//			System.out.println("随机整数:" + random.nextInt(33)); // 0-32 :不包括33 [0-32]
//		}
	}
```



## 八、Date--> java.util.Date

``` xml
类 Date 表示特定的瞬间，精确到毫秒。  1s = 1000毫秒

1.构造方法
	new Date():获取当前系统时间
	new Date(long date):获取指定的时间,从1970,1,1开始计算。0表示

2.普通方法
	getTime(): 返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。
```

``` java
public static void main(String[] args) throws Exception {

		// 1.系统时间
		// while (true) {

		// Thread.sleep(1000); // 睡一秒

		Date date = new Date();
		System.out.println(date);
		long time = date.getTime(); // 毫秒数
		System.out.println(time);

		Date date2 = new Date(time); // 根据毫秒数获取时间
		System.out.println(date2);

		// }

	}
```



### 8.1 DateFormat

``` xml
DateFormat 是日期/时间格式化子类的抽象类,使用的是子类:SimpleDateFormat


```

> | 字母 | 日期或时间元素           | 表示                                 | 示例                                        |
> | ---- | ------------------------ | ------------------------------------ | ------------------------------------------- |
> | `G`  | Era 标志符               | [Text](#text)                        | `AD`                                        |
> | `y`  | 年                       | [Year](#year)                        | `1996`; `96`                                |
> | `M`  | 年中的月份               | [Month](#month)                      | `July`; `Jul`; `07`                         |
> | `w`  | 年中的周数               | [Number](#number)                    | `27`                                        |
> | `W`  | 月份中的周数             | [Number](#number)                    | `2`                                         |
> | `D`  | 年中的天数               | [Number](#number)                    | `189`                                       |
> | `d`  | 月份中的天数             | [Number](#number)                    | `10`                                        |
> | `F`  | 月份中的星期             | [Number](#number)                    | `2`                                         |
> | `E`  | 星期中的天数             | [Text](#text)                        | `Tuesday`; `Tue`                            |
> | `a`  | Am/pm 标记               | [Text](#text)                        | `PM`                                        |
> | `H`  | 一天中的小时数（0-23）   | [Number](#number)                    | `0`                                         |
> | `k`  | 一天中的小时数（1-24）   | [Number](#number)                    | `24`                                        |
> | `K`  | am/pm 中的小时数（0-11） | [Number](#number)                    | `0`                                         |
> | `h`  | am/pm 中的小时数（1-12） | [Number](#number)                    | `12`                                        |
> | `m`  | 小时中的分钟数           | [Number](#number)                    | `30`                                        |
> | `s`  | 分钟中的秒数             | [Number](#number)                    | `55`                                        |
> | `S`  | 毫秒数                   | [Number](#number)                    | `978`                                       |
> | `z`  | 时区                     | [General time zone](#timezone)       | `Pacific Standard Time`; `PST`; `GMT-08:00` |
> | `Z`  | 时区                     | [RFC 822 time zone](#rfc822timezone) | `-0800`                                     |

```` xml
yyyy-MM-dd  2019-03-23

````

| 构造方法                         | 说明                                               |
| -------------------------------- | -------------------------------------------------- |
| SimpleDateFormat()               | 用默认的语言环境的日期格式符号构造SimpleDateFormat |
| SimpleDateFormat(String pattern) | 用给定的模式日期格式符号构造SimpleDateFormat       |

| 方法                      | 说明                           |
| ------------------------- | ------------------------------ |
| String format(Object obj) | 格式化一个对象以生成一个字符串 |
| Date parse(String text)   | 将给定的文本字符串转换为日期   |

```` java
public static void main(String[] args) throws Exception {

		Date mydate = new Date();
		// SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		// SimpleDateFormat format = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
		SimpleDateFormat format = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
		// 1.Date转为指定格式的字符串
		String str = format.format(mydate);
		System.out.println(str);
		// 2.将指定格式的字符串 转为Date
		System.out.println("---------------------------------");

		String strDate = "2019-3-37 14:07:30";
		SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		Date parse = simpleDateFormat.parse(strDate);
		System.out.println(parse);

	}
````



### 8.2Calendar 

| 方法名                   | 说明                              |
| ------------------------ | --------------------------------- |
| getInstance()            | 返回Calendar对象的实例            |
| add(int field,int   val) | 将值val添加指定的时间或日期部分   |
| set(int field,int val)   | 将值val设置到指定的时间或日期部分 |
| get(int field)           | 获取指定的时间或日期部分          |
| getTime()                | 返回Date对象                      |

| 方法                  | 说明                       |
| --------------------- | -------------------------- |
| Calendar.YEAR         | 年份                       |
| Calendar.MONTH        | 月份                       |
| Calendar.DATE         | 日期                       |
| Calendar.DAY_OF_MONTH | 日期，和上面的字段完全相同 |
| Calendar.HOUR         | 12小时制小时数             |
| Calendar.HOUR_OF_DAY  | 24小时制小时数             |
| Calenday.MINUTE       | 分                         |
| Calendar.SECOND       | 秒                         |

``` java
public class Demo05 {

	public static void main(String[] args) {

		Calendar instance = Calendar.getInstance(); // 默认当前系统时间
		System.out.println(instance);

		System.out.println("提取年份:" + instance.get(Calendar.YEAR));
		System.out.println("提取月份:" + (instance.get(Calendar.MONTH) + 1)); // 月份 从0开始计算
		System.out.println("提取日期:" + instance.get(Calendar.DAY_OF_MONTH));
		System.out.println("提取时:" + instance.get(Calendar.HOUR_OF_DAY));
		System.out.println("提取分:" + instance.get(Calendar.MINUTE));
		System.out.println("提取秒:" + instance.get(Calendar.SECOND));
		System.out.println("提取毫秒:" + instance.get(Calendar.MILLISECOND));

		Date date = instance.getTime();

		long m1 = date.getTime(); // 毫秒数
		long m = instance.getTime().getTime(); // 毫秒数

		// Date对象的getTime()用来获取从1970开始的毫秒数
		// Calendar 对象的getTime()获取当前时间Date对象

		// date.getTime()==calendar.getTime().getTime();

		System.out.println("--------------------------------------------------------------------------------------");

		Calendar instance2 = Calendar.getInstance();
		System.out.println(instance2);
		instance2.set(Calendar.YEAR, 9999); // 设置指定字段的时间
		instance2.set(Calendar.MONTH, 9);
		instance2.set(2020, 8, 16, 20, 00); // 年月日时分

		// 添加
		instance2.add(Calendar.MONTH, 5); //当前时间的节点添加5个月
		instance2.add(Calendar.DAY_OF_MONTH, 10); //当前时间 添加 10天

		System.out.println("提取年份:" + instance2.get(Calendar.YEAR));
		System.out.println("提取月份:" + (instance2.get(Calendar.MONTH) + 1)); //月份 从0开始计
		System.out.println("提取日期:" + instance2.get(Calendar.DAY_OF_MONTH));
		System.out.println("提取时:" + instance2.get(Calendar.HOUR_OF_DAY));
		System.out.println("提取分:" + instance2.get(Calendar.MINUTE));
		System.out.println("提取秒:" + instance2.get(Calendar.SECOND));
		System.out.println("提取毫秒:" + instance2.get(Calendar.MILLISECOND));
	}

}
```

