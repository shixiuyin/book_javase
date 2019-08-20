# day03_分支语句

## 一、回顾

``` xml
1.关键字
2.标识符
	自己定义的内容。
	类名:首字符大写
	包名:小写，可以使用.分层
	方法或变量名:驼峰命名 首个单子小写，其余单词首字母大写
	常量:大写,多个单词之间使用下划线 分开
3.变量
	全局变量:
		a.定义范围在类中，方法的外面;
		b.作用域整个类中,可用 ;一对{}中可用
		c.全局变量有初始值。
			byte,short,int,long:0  float,double:0.0 boolean:false char:空'',引用:null
	局部变量:
		a.定义在方法或者代码块中;
		b.作用域只在该放或者代码块内部使用;
		c.使用之前必须赋值;
4.数据类型
	基本数据类型:
		byte,short,int,long ,float,double, boolean,char
	引用数据类型:
		除了基本数据类型以为，都是引用数据类型。默认值都是null。（String,自己的类，Scanner...）
	数据类型的转换:
		自动转换:从小到大
		强制转换:从大到写，丢失精度和数据;
5.运算符
	a.算术运算符
	b.赋值运算符
	c.逻辑运算符
	d.比较运算符
	e.三元运算符
	f.位运算符
	
	a.i++(先赋值，在运算) 和 ++i(先运算，在赋值)
	b.&和&&的区别
		i:&除了是用and之外，还有位运算的功能
		ii:&没有短路的功能。
		iii:&&有短路的功能,避免不必要的判断; 
	c.三元运算符(if...else)
		格式:数据类型  变量  = （条件）?"成功":"失败";
		boolean boolean = (num>20)?true:false;
```

**今天主要内容:**

``` xml
1.if..else
2.switch
```



## 二、分支语句

分支语句根据一定的条件有选择地执行或跳过特定的语句 



## 三、if....else

``` java
if...else主要分为三种实现方式:

1.单分支语句
	只有一个 if(){} 如果满足条件执行。否则直接跳过
		if (score > 90) {
			// 业务
			System.out.println("你很优秀...");
		} 

2.双分支语句
		if (score > 90) {
			// 业务
			System.out.println("你很优秀...");
		} else {
			System.out.println("继续努力....");
		}

3.多分支语句
		if (age > 65) {
			System.out.println("老年");
		} else if (age > 40) {
			System.out.println("中年");
		} else if (age > 17) {
			System.out.println("青年");
		} else if (age > 6) {
			System.out.println("少年");
		} else if (age > 0) { // 以上都不满足 进入else
			System.out.println("童年");
		} else {
			System.out.println("非法输入..");
		}
```

## 四、switch

``` xml
1.语法
	switch(表达式){
		case 常量1:
			语句1;
			break;
		case 常量2:
			语句2;
			break;
		… …
		case 常量N:
			语句N;
			break;
		[default:
			语句;
			break;]
	 } 

2.break
	跳出switch语句块，不继续往下执行;
3.表达式的类型
	目前支持:byte,short,char,int,字符串，枚举
4.case语句内容不能相同。
5.default是可选的。当case都不满足的时候，进入default



```









