# day17_多线程01

## 一、回顾

``` xml
1.程序，进程，线程关系
	程序:为了解决现实生活中的问题编写的一系列有序指令的集合;
	进程:程序的一次执行过程。进程是动态的，程序是静态的；进程之间是独立的。
	线程:进程的基本执行单元,一个进程包含多个线程，线程属于某一个进程。线程可以共享同一个进程中的资源;
	
2.创建线程的方式
	a.继承Thread类，重写run()方法【非必须】
	b.实现Runnable接口,必须重写里面的run()方法

3.线程的方法
	a.start():启动线程，调用run()方法
	b.run():线程需要执行的方法
	c.sleep():睡眠，不会释放对象锁;(占用资源)
	d.join():等待当前线程执行完毕
	e.yield():礼让，主动释放CPU，等待重写被获取资源;
	f.isAlive():判断线程是否活着
	g.interrupt():中断睡眠
	h.getName():获取当前线程的名称
	.....
	j.wait():等待，会释放对象锁
	k.notify():唤醒当前对象被wait的线程;

4.线程的生命周期
	新建:new创建一个对象，但是没有start()启动线程
	可执行(就绪):已经start()启动线程，等待CPU分配资源
	运行:获取CPU资源
	阻塞:sleep(),join(),IO等操作阻塞线程;----阻塞被解决回到:可执行状态
	死亡:run方法执行完毕(正常|抛出异常)


5.线程的调度模型
	分时调度:按照时间片段轮流分配资源;
	抢占式调度:谁抢到了资源，就执行谁。随机
```



## 二、线程的同步

``` xml
1.同步的概念
	同步:同一时刻只能有一个。当前请求没有执行完毕，不能发起其他的请求;
	异步:同时发起多个请求;多个线程同时运行
	线程的同步:同一时刻，只能有一个线程进入公共资源。

2.作用
	为了防止【多个线程】访问【同一个数据对象】时，对数据造成的破坏；

3.通过【锁】实现同步
	java中每一个对象都有一个内置锁，当程序运行到非静态的同步方法上时，自动获得与正在执行代码类的当前实例	（this实例）有关的锁。获取对象锁也叫作对象的同步
	每个对象都只有一个锁，如果一个线程获取了该对象锁，那么在该线程没有释放锁之前，任何线程都无法获取。这	   也就意味着任何线程都不能进入该对象的同步方法和同步代码块

4.同步的实现方式
	隐式锁:synchronized
		确保共享对象在同一时刻只能被一个线程访问，这种处理机制称为“线程同步”或“线程互斥”。Java中的“线程		同步”基于“对象锁”的概念
		
		a.同步代码块
			锁住需要代码块			
			语法: synchronized(obj){//同步代码}
			注意:保证数据安全的情况下，尽可能少的同步代码;

		b.同步方法
			将整个方法进行同步，使用synchronized关键字修饰方法
			public synchronized boolean getApple() {//同步代码}
	

	显示锁:Lock
		自己上锁和解锁，比synchronized更加灵活;
		注意:解锁放在finally块解锁，避免出现异常，没有释放锁对象;
		实现类:
```



### 2.1 同步代码块

``` java
//Runnable:共享资源
public class RunnableThread01 implements Runnable {
	int apple = 5;

	@Override
	public void run() {

		while (true) {
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			synchronized ("xxx") { // 对象是随意的：必须是同一个对象
				if (apple <= 0) {
					break;
				}

				String name = Thread.currentThread().getName();
				System.out.println("当前" + name + " 拿了一个苹果,剩余:" + (--apple));
			}
		}
	}

}


public class Test {
	public static void main(String[] args) {

		RunnableThread01 runable = new RunnableThread01();

		Thread t1 = new Thread(runable, "小强");
		Thread t2 = new Thread(runable, "小花");
		Thread t3 = new Thread(runable, "小明");
		t1.start();
		t2.start();
		t3.start();
	}
}

```



### 2.2 同步方法

``` java
public class RunnableThread02 implements Runnable {

	private int apple = 5;

	@Override
	public void run() {

		while (true) {
			try {
				Thread.sleep(500);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			boolean bool = getApple();
			if (!bool) {
				System.out.println("苹果获取完毕!");
				break;
			}
		}
	}

	// 定义同步的方法 对象锁:this
	public synchronized boolean getApple() {
		if (apple <= 0) {
			return false;// 获取失败
		}
		String name = Thread.currentThread().getName();
		System.out.println("当前" + name + " 拿了一个苹果,剩余:" + (--apple));
		return true;
	}

	public static void main(String[] args) {
		RunnableThread02 runnableThread02 = new RunnableThread02();
		Thread xq = new Thread(runnableThread02, "小强");
		Thread xm = new Thread(runnableThread02, "小名");
		Thread xh = new Thread(runnableThread02, "小化");

		xq.start();
		xm.start();
		xh.start();

	}
}

```

### 2.3 Lock实现同步

``` java
public class RunableThread03 implements Runnable {

	public static void main(String[] args) {
		RunableThread03 runableThread03 = new RunableThread03();
		Thread t1 = new Thread(runableThread03, "老黄");
		Thread t2 = new Thread(runableThread03, "小牛");
		t1.start();
		t2.start();
	}

	// 定义车票
	private int ticket = 100;// 车票

	// 定义锁
	private Lock lock = new ReentrantLock();

	@Override
	public void run() {
		while (true) {

			lock.lock(); //上锁
			try {
				if (ticket <= 0) {
					return;
				}
				System.out.println(Thread.currentThread().getName() + "买了一张票，剩余:" + (--ticket));
			} catch (Exception e) {
			} finally {
				lock.unlock();// 释放锁
			}
		}
	}
}

```



### 2.4同步和锁的注意事项 

``` xml
只能同步方法和代码块，而不能同步变量和类；
每个对象只有一个锁；当提到同步时，应该清楚在什么上同步？也就是说，在哪个对象上同步？
不必同步类中所有的方法，类可以同时拥有同步和非同步方法。
如果两个线程要执行一个类中的synchronized方法，并且两个线程使用相同的实例来调用方法，那么一次只能有一个线程能够执行方法，另一个需要等待，直到锁被释放。也就是说：如果一个线程在对象上获得一个锁，就没有任何其他线程可以进入（该对象的）类中的任何一个同步方法。
如果线程拥有同步和非同步方法，则非同步方法可以被多个线程自由访问而不受锁的限制。
线程睡眠时，它所持的任何锁都不会释放。
线程可以获得多个锁。比如，在一个对象的同步方法里面调用另外一个对象的同步方法，则获取了两个对象的同步锁。
不损害并发性，应该尽可能缩小同步范围。同步不但可以同步整个方法，还可以同步方法中一部分代码块。
在使用同步代码块时候，应该指定在哪个对象上同步，也就是说要获取哪个对象的锁
静态方法的同步，需要一个用于整个类对象的锁，这个对象是就是这个类（XXX.class)

```



### 2.5线程同步小结 

``` xml
1、线程同步的目的是为了保护多个线程访问一个资源时对资源的破坏。
2、线程同步方法是通过锁来实现，每个对象都有且仅有一个锁，这个锁与一个特定的对象关联，线程一旦获取了对象锁，其他访问该对象的线程就无法再访问该对象的其他同步方法。
3、对于静态同步方法，锁是针对这个类的，锁对象是该类的Class对象。静态和非静态方法的锁互不干预。一个线程获得锁，当在一个同步方法中访问另外对象上的同步方法时，会获取这两个对象锁。
4、对于同步，要时刻清醒在哪个对象上同步，这是关键。
5、编写线程安全的类，需要时刻注意对多个线程竞争访问资源的逻辑和安全做出正确的判断，对“原子”操作做出分析，并保证原子操作期间别的线程无法访问竞争资源。
6、当多个线程等待一个对象锁时，没有获取到锁的线程将发生阻塞。
7、死锁是线程间相互等待锁锁造成的，在实际中发生的概率非常的小。但是，一旦程序发生死锁，程序将死掉。
```



## 三、线程的通信|调度(wait)

``` xml
Object中定义了三个方法，可以实现线程的调度
wait（）方法 yield()
	调用了wait方法，会导致本线程放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象发出notify方法后本线程才进入对象锁定池准备获得对象锁进入运行状态。
notify（）方法
	notify的调用不释放锁，只是告诉调用过wait方法的线程可以参与获得锁的竞争了，但不是马上获得锁，要等到获得锁的线程主动释放锁才可。
notifyAll（）
	和notify功能相似，它通知所有调用了wait方法的对象可以参与获得锁的竞争，单它们中只有一个有可能获得锁。


注意:使用wait和notify之前必须使用synchronized。
```



**案例:**

创建两个线程，线程一打印奇数  线程二打印偶数---->依次交替执行

``` java
package com.hzit02;

/**
 * 创建两个线程，线程一打印奇数 线程二打印偶数---->依次交替执行
 * 
 * @author THINK
 *
 */
public class Demo01 {
	public static void main(String[] args) {

		Object obj = "xxx";
		OddRunnable oddRunnable = new OddRunnable(obj);
		EvenRunnable evenRunnable = new EvenRunnable(obj);

		Thread odd = new Thread(oddRunnable, "奇数线程:");
		Thread even = new Thread(evenRunnable, "偶数线程:");

		odd.start();
		even.start();
	}
}

/**
 * 奇数的线程
 * 
 * @author THINK
 *
 */
class OddRunnable implements Runnable {

	private Object obj;

	public OddRunnable(Object obj) {
		this.obj = obj;
	}


	@Override
	public void run() {

		synchronized (obj) { // 同步的基础上使用 wait和notify 进行通信|调度

			for (int i = 1; i <= 100; i++) {
				try {
					Thread.sleep(300);
				} catch (InterruptedException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
				}
				if (i % 2 == 1) {
					System.out.println("奇数线程：" + i);
					obj.notify();// 唤醒obj对象中被 wait的线程
					try {
						if (i != 100) {
							obj.wait(); // 让当前线程等待
						}
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
			}
		}
	}
}

/**
 * 偶数的线程
 * 
 * @author THINK
 *
 */
class EvenRunnable implements Runnable {

	private Object obj;

	public EvenRunnable(Object obj) {
		this.obj = obj;
	}

	@Override
	public void run() {
		synchronized (obj) {

			for (int i = 1; i <= 100; i++) {
				try {
					Thread.sleep(300);
				} catch (InterruptedException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
				}
				if (i % 2 == 0) {
					System.out.println("------------------>>>>>偶数线程：" + i);
					//
					obj.notify();
					try {
						if (i != 100) {
							obj.wait();
						}
					} catch (InterruptedException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
				}
			}
		}
	}
}

```







## 四、生产者和消费者





