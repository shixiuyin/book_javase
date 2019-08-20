# day04_循环语句

## 一、回顾

``` xml
1.if
	单分支
	双分支
	多分支
	if(条件){}  //当满足条件为true的时候，进入语句块;
	如果只有一行代码，可以省略{}

2.switch
	switch...case:
	a.swith数据类型:byte,char,short,int,枚举,字符串(String)
	b.case值必须是常量，不能有重复值
	c.多个case可以共用一个语句块
	d.break 可以跳出语句块，不继续往下执行
	e.default 当所有case都没有进去，进入default。
```

**只要内容**

``` xml
1.for
	普通for,增强for
2.while
	while
	do...while
```



## 二、循环语句

``` xml
功能:
	在满足指定条件下，反复执行特定的代码。

组成部分:
	a.初始化
	b.循环条件
	c.循环体,循环的内容
	d.迭代(改变原来的值)
```



## 三、for循环

``` java
语法:
	for(初始化;条件判断;迭代)
	{
		//循环体 具体反复执行的代码
	}

public static void main(String[] args) {
		for (int i = 1; i <= 5; i++) {
			System.out.println("i=" + i);
		}
	}
```

执行顺序:

![1552525395443](img\1552525395443.png)







## 四、while循环

``` java
语法格式(先判断，再执行):
	[初始值]
	while(条件)
	{
		//语句块
		// 迭代[更改初始值]
	}
	
案例 ：
public static void main(String[] args) {
//		int result = 0;
//		for (int i = 1; i <= 100; i++) {
//				result += i;
//		}
//		System.out.println("result=" + result);

		// while
		int result = 0;
		int i = 1;
		while (i <= 100) { // 判断条件
			// 语句块
			result += i;

			System.out.println("i=" + i + " result=" + result);

			i++;
		}

	}
	
```



do...while

``` java
语法格式:(先执行，在判断:不管条件如何，都会至少执行一次)
	[初始化内容]
    	do{
    		//语句块 
    		[迭代]
    	}while(判断条件);

public static void main(String[] args) {
		int result = 0;
		int i = 1;
		do {
			result += i;
			System.out.println("i=" + i + " result=" + result);
			i++;
		} while (i <= 100);
	}
```



## 五、break,continue

``` xml
1.continue
	跳过本次循环，继续执行下一次循环。
	1234 6789
2.break
	跳出整个循环(默认跳出最近的一个循环)
	1234 

注意:
break只能用于switch语句和循环语句中。
continue 只能用于循环语句中。
标号语句必须紧接在循环的头部。标号语句不能用在非循环语句的前面。
break和continue之后不能有其他的语句，因为程序永远不会执行break 和 continue之后的语句。

```



## 六、嵌套循环

九九乘法表

``` java
public static void main(String[] args) {

		// 1. 外层: 控制行数 1-9行

		// 2.内层 :控制每一行 有多少个列（需要计算多杀次）
		for (int i = 1; i <= 9; i++) {
			// aa:标号 使用:分割 必须写在循环的头部
			 for (int j = 1; j <= i; j++) {
				System.out.print(j + "*" + i + "=" + (j * i) + "\t");
			}
			System.out.println(); // 换行
		}

	}
```













**断点调试**

``` xml
基本步骤:
	1.在需要打断点调试的地方，左侧，双击出现小圆点
	2.使用debug as 启动


```

![1552525045371](img\1552525045371.png)



