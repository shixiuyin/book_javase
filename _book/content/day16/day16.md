# day16_多线程

## 一、程序、进程、线程

``` xml
1.程序（program）
	是为了让计算机执行某些操作或解决某个问题而编写的一系列有序指令的集合 。
2.进程（process）
	是程序的一次执行过程，是系统运行程序的基本单位。程序是静态的，进程是动态的。
	系统运行一个程序即是一个进程从创建、运行到消亡的过程。
3.线程（thread）
	a.比进程更小的运行单位，是程序中单个顺序的流控制。一个进程中可以包含多个线程。
	b.线程是一个独立的执行流，是进程内部的一个独立执行单元，相当于一个子程序
	c.一个进程中的所有线程都在该进程的虚拟地址空间中，使用该进程的全局变量和系统资源。
	d.操作系统给每个线程分配不同的CPU时间片，在某一时刻，CPU只执行一个时间片内的线程，多个时间片中的相应		线程在CPU内轮流执行。

程序，进程，线程的关系:
	 a.进程是程序的一次执行过程。
	 b.线程是进程的基本执行单元，一个进程包含多个线程;

多线程:
	在一个进程中，开辟多个线程同时执行;
	每个Java程序启动后，虚拟机将自动创建一个主线程(main)
```



## 二、多线程的创建方式

``` xml
java提供两种创建线程的方式:
	a.创建 java.lang.Thread 类的子类，重写该类的 run方法
	b.创建 java.lang.Runnable接 口的实现类，实现接口中的 run 方法

run():表示多线程执行的方法,多线程启动自动的去调用run方法;
start():表示线程的启动方法，只有启动该线程，才会用多线程的方式去调用run()方法

```

### 2.1 继承Thread类 

| 构造方法                              | 含义                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| Thread()                              | 创建一个新的线程对象                                         |
| Thread(Runnable target)               | 基于Runnable接口实现类的实例创建一个线程对象                 |
| Thread(Runnable   t,   String   name) | 基于给定的Runnable接口实现类的实例和指定名字创建一个线程对象 |
| Thread(String   name)                 | 基于给定的名称创建一个线程对象                               |

``` java
//继承Thread类，重写run方法
public class MyThread extends Thread {
	@Override
	public void run() {
		String name2 = this.getName();
		System.out.println("当前线程名:" + name2);
		for (int i = 1; i <= 100; i++) {
			System.out.println("线程:" + name2 + " i=" + i);
		}
	}
}


	public static void main(String[] args) {

		String name = Thread.currentThread().getName();
		System.out.println("当前线程名:" + name);
		for (int i = 1; i <= 100; i++) {
			System.out.println("线程:" + name + " i=" + i);
		}

		// 创建新的线程
		MyThread myThread = new MyThread();
		myThread.start(); //开启新的线程
		MyThread myThread2 = new MyThread();
		myThread2.start();
	}

```

### 2.2 实现Runnable接口

``` xml
1.实现Runnable接口创建线程
	a.实现Runable接口;
	b.必须实现里面的run()方法
	c.借助于Thread类，启动的线程
2.线程默认的名称:Thread-xxx

3.Runbable和Thread的区别:
	a.Runnable 接口必须实现 run 方法,Thread可以不重写run方法;
	b.Runnable 接口的实现类并不是真正的线程类，只是线程运行的目标类。要想以线程的方式执行 run 方法，必		须依靠 Thread 类 
	c.Runnable 接口适合于资源的共享

```

``` java

public class RunnableThread01 implements Runnable {
	@Override
	public void run() {
		// 线程执行的方法
		String name = Thread.currentThread().getName();
		for (int i = 1; i <= 100; i++) {
			System.out.println("name:" + name + " i=" + i);
		}
	}
}

	public static void main(String[] args) {

		Runnable target = new RunnableThread01();
		Thread t1 = new Thread(target); // 根据目标的执行方法 创建Thread
		Thread t2 = new Thread(target);
		t1.start();
		t2.start();

		Thread a = new Thread(target, "A线程");
		Thread b = new Thread(target, "B线程");
		a.start();
		b.start();

	}
```



## 三、线程的生命周期

``` xml
1.生命周期
	从出生到死亡过程!
2.线程的生命周期
	线程从创建(start)到死亡的过程。

3.线程的运行状态
	新建(初始化): new 一个线程
	可执行(就绪): start()启动一个线程,可执行(需要等待CPU资源)
	运行(执行):当获取CPU执行权的时候，开始执行;
	阻塞:当前线程阻塞(堵车)。
	死亡:出现异常，强制终止。run()正常执行完毕
```

![1554097252825](img\1554097252825.png)







## 四、线程的调度

``` xml
线程调度
	按照特定机制为线程分配 CPU 时间片段的行为
	Java程序运行时，由 Java 虚拟机负责线程的调度 
线程调度的实现方式
	分时调度模型：让所有线程轮流获得CPU的控制权，并且为每个线程平均分配CPU时间片段
	抢占式调度模型：选择优先级相对较高的线程执行，如果所有线程的优先级相同，则随机选择一个线程执行 。		Java虚拟机采用此种调度模型。

```

### 4.1 线程的优先级

``` xml
分为10级  1-10（最高）
```

