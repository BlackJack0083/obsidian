#### 计算三角形三个角的角度
![[微信截图_20231120205129.png]]
![[微信截图_20231120205145.png]]

### 回顾
![[微信截图_20231120205213.png]]
### 流的概念
- 流就是字节序列的抽象概念
	- 输入流：能*被连续读取数据* 的数据源
	- 输出流：能*被连续写入数据* 的接收端
- I/O流有多种，按操作数据单位不同可分为**字节流(8b)和字符流(16b)**
- 流机制是Java及C++的重要机制，通过流开发人员科研自由控制文件、内存、I/O设备等数据流向
	- I/O流就是用于处理设备上的数据，如硬盘、内存、键盘录入等，如同管道，将两个容器连接起来
![[微信截图_20231120204845.png]]
#### 字节流
- `InputStream`和`OutputStream`都是抽象类，不能实例化
	- 按字节处理数据
```java
package lesson11;
import java.io.*;

public class FileCopyWithStream {
	public static void main(String[] args) throws IOException{
		File srcFile = new File("src/lesson11/FileCopyWithStream.java");
		FileInputStream fis = new FileInputStream(srcFile);
		
		File destFile = new File("backup.java"); 
		// 没有指定路径，这个文件被放在src的上级目录
		FileOutputStream fos = new FileOutputStream(destFile);
		
		byte b[] = new byte[512];  // 创建缓冲区
		int len;
		long begin = System.currentTimeMillis();
		while((len = fis.read(b)) != -1) {  //当读取长度为-1时，表示文件结束
			fos.write(b, 0, len);
			//每次读取时，使用fos.write()将读取的字节写入目标文件
		}
		long end = System.currentTimeMillis();
		System.out.println("copy file spent time: " + (end - begin) + " milliseconds");
		fis.close();
		fos.close();
	}
}
```
*这份代码的作用是复制`FileCopyWithStream`的全部内容，并将其拷贝到`backup.java`的文件中*

1. `File srcFile = new File("src/lesson11/FileCopyWithStream.java");`：创建了一个`File`对象，代表**源文件**。这里的源文件是`src/lesson11/FileCopyWithStream.java`。
2. `FileInputStream fis = new FileInputStream(srcFile);`：创建了一个`FileInputStream`对象，用于从源文件中读取数据。

*Q：为什么需要创建一个`FileInputStream`对象，直接读取不行吗？*
创建`FileInputStream`对象是为了**与文件进行交互**，它提供了读取文件内容的方法。在Java中，**不能直接读取文件内容**，需要通过**输入流**来实现。`FileInputStream`是Java I/O库中的一个类，专门用于从文件中读取数据。当你创建一个`FileInputStream`对象并关联到一个文件时，你就打开了一个指向该文件的输入流，然后可以通过这个输入流来读取文件中的数据。

3. `File destFile = new File("backup.java");`：创建了一个`File`对象，代表**目标文件**。这里的文件名是`backup.java`，并且因为它没有指定路径，所以它将被创建在源文件的同级目录下。
4. `FileOutputStream fos = new FileOutputStream(destFile);`：创建了一个`FileOutputStream`对象，用于将数据写入目标文件。

5. `byte b[] = new byte[512];`：创建了一个字节数组，作为**缓冲区**。这个缓冲区的大小是512字节。

*Q：什么是缓冲区？为什么要使用`byte`数组作为缓冲区？数组长度对缓冲区有什么影响？*
 在文件复制过程中，使用byte数组作为缓冲区可以提高效率。缓冲区是一个存储数据的临时区域，它位于内存中，用于在读取和写入操作之间暂存数据。

使用byte数组作为缓冲区的好处有以下几点：
1. **减少I/O次数**：如果每次只读取或写入一个字节，那么对于大文件来说，将需要非常多次的I/O操作。通过使用缓冲区，我们可以*一次性读取或写入多个字节*，从而大大减少I/O次数。
2. **提高效率**：由于*减少了I/O次数*，所以整体的文件复制速度会得到提高。
3. **灵活性**：使用缓冲区*可以更容易地处理各种数据块的大小*。

数组的长度确实会对缓冲区有影响：
* **如果数组长度太小**：例如，如果数组长度只有1字节，那么缓冲区的优势就几乎不存在了，因为每次只能处理一个字节。这样会增加I/O操作的次数，从而降低效率。
* **如果数组长度适中**：例如，常见的选择是512字节或1024字节。这样的缓冲区大小可以在大多数情况下提供很好的性能。
* **如果数组长度太大**：虽然大缓冲区可以减少I/O次数，但它也会占用更多的内存。如果缓冲区过大，可能会导致内存不足的问题，尤其是在处理多个大文件或同时运行其他内存密集型应用时。

3. `int len;`：声明了一个整数变量，用于存储从源文件中*读取的字节数*。
4. `long begin = System.currentTimeMillis();`：获取当前时间的毫秒数，并存储在变量`begin`中。这用于计算文件复制所需的时间。
5. `while((len = fis.read(b)) != -1) { ... }`：这是一个循环，用于从源文件中读取数据，直到文件的末尾。每次循环，它都尝试从源文件中读取最多512个字节的数据到缓冲区`b`中，并将实际读取的字节数存储在`len`中。当读取到文件的末尾时，`read()`方法返回-1，循环结束。
6. `fos.write(b, 0, len);`：这行代码在循环内，将从源文件中读取的数据（存储在缓冲区`b`中）写入目标文件。它仅写入实际读取的字节数（即`len`个字节）。
7. `long end = System.currentTimeMillis();`：在文件复制完成后，再次获取当前时间的毫秒数，并存储在变量`end`中。
8. `System.out.println("copy file spent time: " + (end - begin) + " milliseconds");`：计算并打印文件复制所需的时间（以毫秒为单位）。

#### 过滤流
![[微信截图_20231120205457.png]]
```java
File fo = new File(strPath);
FileInputStream fos = new FileInputSstream(fo);
BufferOutputStream bos = new BufferOutputStream(fos);
DataOutputStream dos = new DataOutputStream(bos);
```
#### IO流之间的转换
![[微信截图_20231120205827.png|500]]
```java
File file = new File(strPath);
FileInputStream fis = new FileInputStream(file);
BufferedInputStream bis = new BufferedInputStream(fis);
```
##### DataInputStream类用法
```java
import java.io.*;
public class FormatDataIO{
	public static void main(String[] args)throws IOException{
		FileOutputStream out1 = new FileOutputStream("D:\\test.txt");
		BufferedOutputStream out2 = new BufferedOutputStream(out1); // 装饰文件输出流
		DataOutputStream out = new DataOutputStreaam(out2);
		out.writeByte(-12);
		out.writeLong(12);
		out.writeChar('1');
		out.writeUTF("好");
		out.close();

		InputStream in1 = new FIleInputStream("D:\\test.txt");
		BufferedInputStream in2 = new BufferedInputStream(in1); // 装饰文件输入流
		DataInputStream in = new DataInputStream(in2); // 装饰缓冲输入流
		System.out.print(in.readByte()+" ");
		System.out.print(in.readLong()+" ");
		System.out.print(in.readChar()+" ");
		System.out.print(in.readUTF()+" ");
		in.close();
	}
}
```
#### 字符流
![[微信截图_20231120211306.png|500]]
##### Reader/Writer
![[微信截图_20231120211451.png|500]]
### 控制台I/O
#### 打印命令重载
- `System.out`对象默认是计算机屏幕
	- `println`或`print`方法都对8种基本类型进行重载
	- 重载了`char[]`和`Object`
```java
import java.io.*;
public class TestOutput {
	public static void main(String[] args) {
		char c[] = {'a', 'b', 'c'};
		System.out.println(c); // abc
		int d[] = {1,2,3};
		System.out.println(d); // 打印地址
	}
}
```

#### 在键盘实现屏幕输出
```java
import java.io.*;

public class TestInput {
	public static void main(String[] args) {
		InputStreamReader ir = new InputStreamReader(System.in);
		BufferedReader in = new BufferedReader(ir);
		System.out.println("Ctrl + z to exit");
		try {
			String s = in.readLine();
			while(s != null) {
				System.out.println("Read: " + s);
				s = in. readLine();
			}
			in.close();
		}catch(IOException e) {
			e.printStackTrace();
		}
	}
}
```

### 文件类
![[微信截图_20240102143450.png]]
#### File
管理文件系统
![[微信截图_20240102143545.png]]
```java
import java.io.*;
impirt java.io.Date;
public class DirUtil{
	public static void main(String args[])throws Exception{
		File dir1=new File("D:\\dir1");
		if(!dir1.exists())dir1.mkdir();
		File dir2=new File(dir1,"dir2");
		if(!dir2.exists())dir2.mkdirs();
		File dir4=new File(dir1,"dir3\\dir4");
		if(!dir4.exists()) dir4.mkdirs();
		File file=new File(dir2,"test.txt");
		if(!file.exists())file.createNewFile();
		listDir(dir1);
		deleteDir(dir1);
	}
}
```
#### PrintWriter
![[微信截图_20240102143719.png]]
#### Scanner
![[微信截图_20240102143740.png]]
#### 文件读入屏幕输出
- 文件输入：
	- `FileReader`读字符；
	- `BufferedReader`：`readLine()`方法
- 文件输出：
	- `FileWriter`写字符；
	- `PrintWriter`：`print()`和`println()`方法
```java
import java.io.*;
public class ReadFile{
	public static void main(String args[]){
	File file = new File(args[0]);
	try{
		BufferedReader in=new BufferedReader(new FileReader(file));
		String s;
		while((s = in.readLine())!=null){
		System.out.println("Read:"+s);
		}
		in.close();
	}
	catch(FileNotFoundException e1){System.err.println("File not found" + file);}
	catch(IOException e2){e2.printStackTrace();}
	}
}
```
#### 写入文件
```java
import java.io.*;

public class TestInput {
	public static void main(String[] args) {
		File file = new File("testinput.txt");
		
		try {
			InputStreamReader ir = new InputStreamReader(System.in);
			BufferedReader in = new BufferedReader(ir);
			PrintWriter out = new PrintWriter(new FileWriter(file));
			String s;
			System.out.println("Enter file text and ctrl + z to exit");
			while((s=in.readLine()) != null) {
				out.println(s);
			}
			in.close();
			out.close();
		}catch(IOException e) {
			e.printStackTrace();
		}
	}
}
```
#### 键盘输入文件输出
```java
import java.io.*;
public class WriteFile{
	public static void main(String args[]){
		File file = new File(args[0]);
		try{
			BufferedReader in=new BufferedReader(new
			InputStreamReader(System.in));
			PrintWriter out = new PrintWriter( new FileWriter(file));
			String s;
			System.out.println("Enter file text and ctrl+z to exit");
			while((s = in.readLine())!=null){out.println(s);}
			in.close(); out.close();
		}catch(IOException e){
			e.printStackTrace();
		}
	}
}
```
#### 从WEB读入数据
`URL`类
![[微信截图_20240102145409.png]]

### 文本替换
`ReplaceText`将一个字符串用新字符串替换。字符串和文件名通过命令行参数传递。
```java
import java.io.*; import java.util.*;
public class ReplaceText {
	public static void main(String[] args) throws Exception {
	if (args.length != 4) {
		System.out.println("Usage: java ReplaceText sourceFile targetFile oldStr newStr");
		System.exit(1);
	}
	File sourceFile = new File(args[0]);
	if (!sourceFile.exists()) {
		System.out.println("Source file " + args[0] + " does not exist"); System.exit(2);
	}
	File targetFile = new File(args[1]);
	if (targetFile.exists()) {
		System.out.println("Target file " + args[1] + " already exists and replaced");}
	else {
		System.out.println("A new file is created with replaced text!");
	}
	String str1 = args[2]; String str2 = args[3];
	try(Scanner input = new Scanner(sourceFile); PrintWriter output = new PrintWriter(targetFile);)
	{
			while (input.hasNext()) {
			String s1 = input.nextLine();
			String s2 = s1.replaceAll(str1, str2);
			output.println(s2);
			}
		}
	}
}
```
## 对象序列化
- Java程序运行过程中创建了一系列对象，而且对象的状态也进行了设置，如果程序结束，则所有对象都将被垃圾回收而销毁
- 对象序列化提供了一种技术将对象与硬盘文件互相转化
	- 把**对象转换为字节序列**的过程称为**对象的序列化**（对象打散为字节）
	- 把字节序列恢复为对象的过程称为对象的反序列化（字节结构化为对象）
- 两种用途：
	- 把对象的字节序列**永久地保存**到硬盘上，通常存放在一个文件中
	- 在**网络上传送对**象的字节序列
	- 在很多应用中，需要对某些对象进行序列化，让它们离开内存空间，入住物理硬盘，以便长期保存
### JDK类库中的序列化API
- `java.io.ObjectOutputStream`代表对象输出流
	- 其方法`writeObject(Object obj)`方法可对参数指定的obj对象进行序列化，把得到的字节序列写到一个目标输出流中
- `java.io.ObjectInputStream`代表对象输入流
	- 其方法`readObject()`方法从一个源输入流中读取字节序列，再把它们反序列化为一个对象，并将其返回
- 方法不需要序列化，只有属性需要序列化

### 小结
![[微信截图_20240102150006.png]]

## 装饰器设计模式
>装饰器设计模式是一种设计模式，用于在不改变对象自身的基础上，动态地给对象添加额外的职责或功能。这种设计模式提供了比继承更有弹性的替代方案，可以在运行时动态地扩展一个对象的功能。

- 假设有一个餐厅出售快餐
	- 大类：米饭(Rice)，面条(Noodle),...
	- 配菜装饰：蛋炒(EggDeco)，培根(BaconDeco)
	- 每种快餐都有方法：
		- `getPrice()`返回不同价格
		- `cook()`
- 如何设计类间关系
![[微信截图_20231120205556.png|500]]
```java
abstract class FastFood {
	private int price;
	public abstract void cook();
	public FastFood(int price){
		this.price = price;
	}
	public int getPrice() {return price;}
}
class Noodle extends FastFood{
	public Noodle(int price) {
		super(price);
	}
	public int getPrice() {return super.getPrice();}
	public void cook() {
		System.out.println("cook noodles");
	}
}
class Rice extends FastFood{//炒饭类
	public Rice(int price) {
		super(price);
	}
	public int getPrice() {return super.getPrice();}
	public void cook() {
		System.out.println("fried rice");
	}
}

abstract class SideDish extends FastFood{ //配菜类
	private FastFood fastFood;  // 组合
	public SideDish(FastFood fastFood, int price) {
		super(price);
		this.fastFood = fastFood;
	}
	public int getPrice() {
		return (fastFood.getPrice() + super.getPrice());
	}
	public void cook() {
		fastFood.cook();
	}
}
class EggDeco extends SideDish {
	public EggDeco(FastFood fastFood, int price) {
		super(fastFood, price);
	}
	public void cook() {
		super.cook();
		System.out.println("add fried eggs");
	}
}
class BalconDeco extends SideDish {
	public BalconDeco(FastFood fastFood, int price) {
		super(fastFood, price);
	}
	public void cook() {
		super.cook();
		System.out.println("add salty balcon");
	}
}
public class TestDecorator {
	public static void main(String[] args) {
		FastFood rice = new Rice(10);//一开始点了一碗炒饭
		rice.cook();
		System.out.println("price for rice :" + rice.getPrice());
		System.out.println("------------------------");
		FastFood noodle = new Noodle(8);//一开始点了一碗面条
		noodle.cook();
		System.out.println("price for noodle :" + noodle.getPrice());
		System.out.println("------------------------");
		FastFood eggDeco = new EggDeco(rice, 2);//加个蛋
		eggDeco.cook();
		System.out.println("price for rice with fried egg:" + eggDeco.getPrice());
		System.out.println("------------------------");
		BalconDeco balcony = new BalconDeco(rice, 3);//加个肠
		balcony.cook();
		System.out.println("price for rice with salty balcon:" + balcony.getPrice());
		System.out.println("------------------------");
		eggDeco = new EggDeco(noodle, 2);//加个蛋
		eggDeco.cook();
		System.out.println("price for noodle with fried egg:" + eggDeco.getPrice());
		System.out.println("------------------------");
		balcony = new BalconDeco(noodle, 3);//加个肠
		balcony.cook();
		System.out.println("price for noodle with salty balcon:" + balcony.getPrice());
	}
}
```
基本操作：继承抽象类/接口+组合
```java
public abstract class Decorator implements Component {
    protected Component component;

    public Decorator(Component component) {
        this.component = component;
    }

    public void operation() {
        component.operation();
        // 可以添加额外的操作
    }
}

```
注意：
![[微信截图_20240102150116.png]]

![[微信截图_20240102150209.png]]
a) `BufferedInputStream`: 是的，`BufferedInputStream` 的构造方法可以接受 `InputStream` 类型的参数，用于包装另一个输入流并提供缓冲功能。

b) `DataInputStream`: 是的，`DataInputStream` 的构造方法可以接受 `InputStream` 类型的参数，用于包装另一个输入流，并提供用于读取基本数据类型的方法。

c) `SequenceInputStream`: 是的，`SequenceInputStream` 的构造方法可以接受两个 `InputStream` 类型的参数，用于合并这两个输入流。

d) `FileInputStream`: 不是。`FileInputStream` 的构造方法接受 `String` 或 `File` 类型的参数，用于指定要打开的文件路径或文件对象，而不是直接接受 `InputStream` 类型的参数。

![[微信截图_20240102150225.png]]

![[微信截图_20240102150234.png]]
