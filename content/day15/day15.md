# day14_io流

## 一、回顾

``` xml
1.Collections
	List:有序，可重复
		ArrrayList:底层数组，不安全,查询效率高;
		Vector:底层数组，线程安全,查询效率高;
		LinkedList:底层使用双向列表,添加和删除效率高，线程不安全;

	Set:无序，不可重复
		HashSet: hash值决定存放的位置,无序，不可重复
		LinkedHashSet:是HashSet的子类，多添加了链表，具有插入的顺序，不可重复
		TreeSet:默认按照自然顺序排序，指定比较器


2.Map
	HashMap:键值对，key-value的集合  Entry==<key,value> ,线程不安全，key无序，可以存放null
	HashTable:线程安全，不可以存放null;
	LinkedHashMap:key是有插入顺序
	Properties:属性文件，key,value都是String类型
	TreeMap:key按照自然顺序排序
        
     Map的遍历方式:
        a.先获取key,通过key回去value;
        b.获取整个Entry,得到里面的key,value(推荐使用)


3.Iterator
	a.创建迭代器,Collection 里面的方式  iterator()
	b.hasNext()判断是否还有元素
	c.next()获取元素;

4.泛型
	a.让集合记住具体的数据类型,默认丢失数据类型

5.比较器
	a.实体类去实现 Comparable接口, 重写里面的compareTo(T o)方法
	b.创建单独的比较器 Comparator,重写里面的 int compare(T o1,T o2)

6.工具类
	Colletions里面有很多提供好的方法;
```



## 二、IO流

``` xml
1.流的分类
	按照流向划分:
		输入流:以内存为基础，从外部设备(磁盘)到内存中称为输入流;
		输出流:从内容到外部设备称为输出流;
	按照单位划分:
		字节流:以字节为单位。（万能流,操作任意的文件(文本,图片,视频,压缩包....)）
		字符流:以字符为单位，只能操作文本数据
	字节输入流:
	字节输出流:
	字符输入流:
	字符输出流:

2.概述
	含义:流是指一连串流动的字符,是以先进先出方式发送信息的通道
	应用场景:打印机,文件上传和下载,文件的复制粘贴;
	
```

## 三、File

``` xml
1.概述
	a.File 类代表与平台无关(window,linux)的文件和目录。
	b.磁盘中只有两种东西:文件和文件夹(目录),文件有不同的类型。通过后缀区分
	c.File能新建、删除、重命名文件和目录,不能访问文件中具体内容。需要(输入输出流)

2.常用方法
	访问文件名:
		getName():获取文件名
		getPath():获取文件路径名
		getAbsolutePath():获取文件绝对路径名
		getAbsoluteFile():获取文件绝对路径名File对象
		getParent():父文件的名称,没有返回null
		getParentFile():父类文件的对象,没有返回null
		renameTo():文件的重命名
	
	文件检测:
		exists():判断文件和目录是否存在
		canRead():判断文件是否可读
		canWrite():判断文件是否可写
		isFile():判断是否是一个文件
		isDirectory():判断是否是一个文件夹(目录)

	获取文件的常用信息:
		lastModified():获取最后一个修改时间
		length():获取文件的大小
	
	文件相关操作：
		createNewFile():新建文件
		delete():删除文件

	目录相关的操作:
		mkdir():创建目录，创建当前层级目录;
		mkdirs():创建目录，如果父目录不存在，也会同时创建;
		list():获取该目录下所有的文件和目录的名称
		listFiles():获取该目录下所有的文件和目录的File对象

3.构造方法
	File(String pathname):文件或者文件夹的名称
	File(String parent, String child):字符串类的文件夹和文件的名称
	File(File parent, String child):File对象的文件和child字文件和目录
```

``` java
//构造方法使用
public static void main(String[] args) {
		// 根据获取对象
		File file = new File("d://bj1901//Test01.java");
		System.out.println(file.getAbsolutePath());
		System.out.println(file.lastModified());

		File file2 = new File("d://bj1901", "Test01.java");
		System.out.println(file2.lastModified());

		File parent = new File("d://bj1901");
		File file3 = new File(parent, "Test01.java");
		System.out.println(file3.lastModified());

	}
```

## 四、IO流体系

![1553754774253](img\1553754774253.png)

![1553754799685](img\1553754799685.png)



### 4.1 字节输入流InputStream

``` xml
1.InputStream根
	此抽象类是表示字节输入流的所有类的超类。 

2.常用的实现类
	FileInputStream: 从文件系统中的某个文件中获得输入字节
	BufferedInputStream:缓冲流,作用提高效率
	DataInputStream:二进制流
	ObjectInputStream:对象流，用于反序列化

3.常用的方法
	read():从输入流中读取数据的下一个字节,-1表示读取完毕;
	read(byte[] b)
	read(byte[] b,int off,int len);
```

![1553755669590](img\1553755669590.png)

``` JAVA
//读取文件
		int len = -1; //存放读取的长度  默认-1
		byte[] bytes = new byte[1024];
		// int read = inputStream.read(b); // 返回值读取的长度
		while ((len = inputStream.read(bytes)) != -1) { // 执行 添加数据到bytes数组
			// 将数组转为字符串
			// String string = new String(bytes);
			String string = new String(bytes, 0, len, "UTF-8");
			System.out.println(string);
		}
```



### 4.2 字节输出流OutputStream

``` 
1.概述
	此抽象类是表示输出字节流的所有类的超类;
2.实现类
	FileOutputStream: 从文件系统中的某个文件中获得输入字节
	BufferedOutputStream:缓冲流,作用提高效率
	DataOutputStream:二进制流
	ObjectOutputStream:对象流，用于反序列化

3.方法
```

![1553757691700](img\1553757691700.png)

``` java
	public static void main(String[] args) throws IOException {
		OutputStream outputStream = new FileOutputStream("D:\\bj1901\\text.txt", true); // true:表示追加文本，而不是覆盖

		// 需要输入的内容，转为字节
		String str = "ASP.NET不错";
		byte[] bytes = str.getBytes();

		outputStream.write(bytes);

		outputStream.flush();// 刷新
		outputStream.close();
		System.out.println("输出成功...");

	}
```



#### 1.复制文件

``` java

/**
 * 复制文件
 * 
 * @author THINK
 *
 */
public class Demo01 {
	public static void main(String[] args) throws Exception {
		copy02("D:\\开发工具\\win32_11gR2_client.zip", "C:\\");
	}
	/**
	 * 复制文件:一边读取 一边写
	 * 
	 * @param sources 源文件的路径
	 * @param target  需要复制到哪个目录
	 * @throws IOException
	 */
	public static void copy(String sources, String target) throws IOException {
		// 1.创建file对象
		File in = new File(sources);

		// 1.1 获取源文件的名称
		String name = in.getName();
		File out = new File(target, name);
		// 2.根据file创建对应的流
		FileInputStream inputStream = new FileInputStream(in); // 读取源文件
		FileOutputStream outputStream = new FileOutputStream(out);// 输出到指定的文件

		// 3.读取(一遍读取，一遍写入 )
		byte[] bytes = new byte[1024];
		int len = -1;
		while ((len = inputStream.read(bytes)) != -1) {
			// 4.写到目标文件中
			outputStream.write(bytes, 0, len);
		}

		// 5.关闭资源
		outputStream.flush();
		outputStream.close();
		inputStream.close();
		System.out.println("复制成功:" + out.getAbsolutePath());

	}

	/**
	 * 复制文件:先将文件流 读取到集合 从集合写入文件
	 * 
	 * @param sources
	 * @param target
	 * @throws IOException
	 */
	public static void copy02(String sources, String target) throws IOException {

		List<byte[]> list = new ArrayList<>();

		// 1.创建file对象
		File in = new File(sources);

		// 1.1 获取源文件的名称
		String name = in.getName();
		File out = new File(target, name);
		// 2.根据file创建对应的流
		FileInputStream inputStream = new FileInputStream(in); // 读取源文件
		FileOutputStream outputStream = new FileOutputStream(out);// 输出到指定的文件

		// 3.读取(一遍读取，一遍写入 )
		byte[] bytes = new byte[1024];
		int len = -1; // 20
		while ((len = inputStream.read(bytes)) != -1) {
			if (len != bytes.length) // 最后一次读取 数组没有读满
			{
				// 创建新的数组 存放最后读取的内容
				byte by[] = new byte[len];
				//by = bytes;
				for (int i = 0; i < by.length; i++) {
					by[i] = bytes[i];
				}
				list.add(by);
			} else {
				list.add(bytes);
			}
			bytes = new byte[1024]; // 新建数组
		}

		// 4.写到目标文件中
		for (byte[] b : list) {
			outputStream.write(b);
		}

		// 5.关闭资源
		outputStream.flush();
		outputStream.close();
		inputStream.close();
		System.out.println("复制成功:" + out.getAbsolutePath());

	}

}


```





### 4.3 字符输入流Reader

``` xml
1.概述 
	用于读取字符流的抽象类
```

``` java
public class Demo01 {

	public static void main(String[] args) throws IOException {
		Reader reader = new FileReader("D:\\bj1901\\hello.txt");

		char ch[] = new char[5]; // 字符 和 字节使用方式一样
		int len = -1;
		while ((len = reader.read(ch)) != -1) {
			String string = new String(ch, 0, len);
			System.out.println(string);
		}
		System.out.println("长度:" + len);

	}
}
```

#### 4.3.1 字符输入缓冲流

字符输入流中读取文本，缓冲各个字符，从而实现字符、数组和行的高效读取。 

如果读取到文档的末尾，返回null

优点:提高效率，可以直接读取一行;

``` java
public class Demo02 {

	public static void main(String[] args) throws IOException {
		Reader reader = new FileReader("D:\\bj1901\\hello.txt");

		BufferedReader bufferedReader = new BufferedReader(reader);
		String temp = null;
		while ((temp = bufferedReader.readLine()) != null) {
			System.out.println(temp);
		} // 如果读取到文档的末尾，返回null
        
		reader.close();
		bufferedReader.close();
	}
}
```





### 4.4 字符输出流

``` xml
Writer:字符输出流
	
提供的方法:
	Writer append(char c) 
          将指定字符添加到此 writer。 
	Writer append(CharSequence csq) 
          将指定字符序列添加到此 writer。 
 	Writer append(CharSequence csq, int start, int end) 
          将指定字符序列的子序列添加到此 writer.Appendable。 
	abstract  void close() 
          关闭此流，但要先刷新它。 
	abstract  void flush() 
          刷新该流的缓冲。 
 	void write(char[] cbuf) 
          写入字符数组。 
	abstract  void write(char[] cbuf, int off, int len) 
          写入字符数组的某一部分。 
 	void write(int c) 
          写入单个字符。 
 	void write(String str) 
          写入字符串。 
 	void write(String str, int off, int len) 
          写入字符串的某一部分。 

```

通过字符和字符缓冲流复制文件：

``` java
public class Demo03 {

	public static void main(String[] args) throws Throwable {
		copy02("d://bj1901//test.txt", "C://");
	}

	/**
	 * 字符流
	 * 
	 * @param sources
	 * @param target
	 * @throws Exception
	 */
	public static void copy(String sources, String target) throws Exception {

		File in = new File(sources);
		File out = new File(target, in.getName());

		Reader reader = new FileReader(in);
		FileWriter fileWriter = new FileWriter(out);

		char[] ch = new char[1024];
		int len = -1;
		while ((len = reader.read(ch)) != -1) {
			fileWriter.write(ch, 0, len);
		}

		fileWriter.flush();
		fileWriter.close();
		reader.close();
		System.out.println("复制成功:" + out.getAbsolutePath());

	}

	public static void copy02(String sources, String target) throws Exception {

		File in = new File(sources);
		File out = new File(target, in.getName());

		Reader reader = new FileReader(in);
		FileWriter fileWriter = new FileWriter(out);

		// 1.得到字符缓冲流
		BufferedReader bufferedReader = new BufferedReader(reader);
		BufferedWriter bufferedWriter = new BufferedWriter(fileWriter);

		String str = null;
		while ((str = bufferedReader.readLine()) != null) {
			bufferedWriter.write(str);
			bufferedWriter.newLine(); // 换行
		}
		bufferedWriter.flush();
		bufferedWriter.close();
		reader.close();
		System.out.println("复制成功:" + out.getAbsolutePath());
	}
}
```



### 4.5 转换流

``` xml
字节流：万能流
字符流:只能操作文本字符，操作字符的效率比万能流更高;

字节流转为字符流:

字节输入流---->字符输入流:InputStreamReader
字节输出流---->字符输出流:OutputStreamWriter
```



#### 1.InputStreamReader

``` xml
1.概述
	InputStreamReader 是字节流通向字符流的桥梁；
	它使用的字符集可以由名称指定或显式给定，或者可以接受平台默认的字符集；

2.构造方法
	InputStreamReader(InputStream in):默认字符集
	InputStreamReader(InputStream in, String charsetName):指定字符集

3.常用方法
```

![1553839895542](img\1553839895542.png)

``` java
// 以后很多平台:只提供获取字节的方法
	public static void main(String[] args) throws IOException {

		// 1.准备字节流
		// FileInputStream inputStream = new FileInputStream("d://bj1901//Right.txt");

		// 2.转成成字符流
		// Reader inputStreamReader = new InputStreamReader(inputStream);

		// 2.字符流转为缓冲流
		// BufferedReader bufferedReader = new BufferedReader(inputStreamReader);

		//三句换成一句
		BufferedReader bufferedReader = new BufferedReader(
				new InputStreamReader(new FileInputStream("d://bj1901//Right.txt")));

		// 3.获取流信息
		String temp = null;
		while ((temp = bufferedReader.readLine()) != null) {
			System.out.println(temp);
		}

		// 关闭资源
		bufferedReader.close();
		// inputStreamReader.close();
		// inputStream.close();

	}
```



#### 2.OutputStreamWriter

``` xml
1.概述
	OutputStreamWriter 是字符流通向字节流的桥梁
```

``` java
public class Demo02 {

	public static void main(String[] args) throws IOException {
		String str = "我是中国人abcdg";

		// 1.字节
		FileOutputStream outputStream = new FileOutputStream("d://bj1901//Right.txt", true);
		OutputStreamWriter outputStreamWriter = new OutputStreamWriter(outputStream);

		//缓冲流 不是必须的 
		BufferedWriter bufferedWriter = new BufferedWriter(outputStreamWriter);

		bufferedWriter.newLine();
		bufferedWriter.write(str);
		bufferedWriter.flush();
		bufferedWriter.close();
		System.out.println("成功....");
	}
}
```



### 4.6 对象的序列化

``` xml
1.概述
	a.对象序列化的目标是将对象保存到磁盘上，或允许在网络中直接传输对象。
	b.序列化是 RMI（Remote Method Invoke – 远程方法调用）过程的参数和返回值都必须实现的机制

2.实现序列化的方式
	类通过实现 java.io.Serializable 接口以启用其序列化功能
```

#### 1.序列化 和返序列化

``` xml
将内存中的对象序列化到磁盘,是一个输出流

ObjectOutputStream:
	方法:void writeObject(Object obj)  
```



``` java
public class Demo03 {

	public static void main(String[] args) throws Exception {
		//ser();
		revser();
	}

	// 序列号
	public static void ser() throws FileNotFoundException, IOException {
		// 对象
		Person person = new Person("zhangsan", 12);

		// 集合 ALL
		ObjectOutputStream outputStream = new ObjectOutputStream(new FileOutputStream("D://bj1901//zhangsan.data"));
		outputStream.writeObject(person);
		outputStream.flush();
		outputStream.close();
		System.out.println("序列化成功....");
	}

	// 返序列号
	public static void revser() throws Exception {
		
		ObjectInputStream objectInputStream = new ObjectInputStream(new FileInputStream("D://bj1901//zhangsan.data"));
		Person person = (Person)objectInputStream.readObject();
		System.out.println(person.getName());
		objectInputStream.close();
	}

}

```







