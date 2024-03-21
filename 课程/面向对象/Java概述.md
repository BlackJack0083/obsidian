**方法：**
	对于现实世界的复杂系统，如何分析，如何设计，如何建模，统一建模语言(UML)
	面向对象方法是运用**类、对象、继承、封装、重载、多态、关联、消息**等概念来构造系统的软件开发方法
	**设计范式**：经过反复使用的代码设计经验，每种模式描述了一个在我们周围不断重复发生的问题，以及该问题的核心解决方案
**技术：**
	用编程语言来实现面向对象的系统建模的知识与技能；  
	Java编程是技术层面，掌握了一门语言是掌握了一种技术，Java是纯面向对象的编程语言
# 概论
## 计算机基本组成
![[微信截图_20230904182314.png|500]]
### CPU
**计算机核心**(central processing unit)
从内存中提取并处理命令
衡量处理速率：megahertz(MHz，兆赫兹)， 1 MHz = 1 million pulses/second
### Memory(内存)
**存储数据并编写指令**(store data and program instructions)
一个字节串规划了一个内存单元，**每个字节包含8比特(bits)**
*在所有程序和数据被处理前必须被加入内存*
>A memory unit is an ordered sequence of *bytes*, each holds *eight bits*. A program and its data must be brought to memory before they can be executed.  
>A memory byte is *never empty*, but its initial content may be  meaningless to your program. The current content of a memory byte  is lost whenever new information is placed in it

#### 数据的存储方式(二进制代码)
![[微信截图_20230904184240.png|500]]
### 存储设备(Storage Devices)
![[微信截图_20230904184454.png|500]]
内存易变，因为当电源关闭时里面的数据就消失了
程序和数据往往*永久性*存储在存储设备中并且当计算机要调用时才会移动到内存中

>内存（RAM）和硬盘（硬盘驱动器）是计算机系统中的两种不同类型的存储设备，它们在功能和工作方式上有很大的区别。
>1. **内存（RAM - Random Access Memory）：**
   - **作用：** 内存是计算机用于*临时存储数据和程序*的地方。当计算机启动时，操作系统和正在运行的程序都会加载到内存中。
   > -  **速度：** 内存的读写速度非常快，可以迅速存取数据，是CPU直接访问的地方。
   - **易失性：** 内存是*易失性存储*，也就是说，当计算机关闭或重启时，内存中的数据将被清除。

>2. **硬盘（硬盘驱动器）：**
   - **作用：** 硬盘是计算机中用于长期存储数据和程序的设备。它保存操作系统、应用程序、用户文件等。
   > -  **速度：** 相对于内存，硬盘的读写速度较慢。硬盘通常是机械设备，需要一定的时间来寻找和读取数据。
   - **非易失性：** 硬盘是*非易失性存储*，即使在关机时数据也会被保留。

>**区别：**
   - **速度：** 内存速度快，适合需要快速访问的临时数据；硬盘速度慢，适合长期存储。
>    - **易失性：** 内存是易失性存储，关机时数据丢失；硬盘是非易失性存储，数据持久保存。
   - **用途：** 内存用于存放正在运行的程序和临时数据；硬盘用于存放操作系统、应用程序和用户文件等长期存储的数据。
>    -  **成本：** 一般情况下，内存的价格比硬盘高。
> 总的来说，内存和硬盘在计算机系统中扮演不同的角色，相互协同工作以实现计算机的正常运行。
### 输出设备(Output Devices)
>The monitor displays information (text and graphics). The resolution  and dot pitch determine the quality of the display
### 通信设备(Communication Devices)

### 操作系统(Operation Systems)
![[微信截图_20230912091013.png|500]]

## 机器语言、汇编语言与高级语言
>Computer programs, known as *software*, are instructions to the computer.  
>You tell a computer what to do through programs. Without programs, a computer is an empty machine. Computers do not understand human languages, so you  need to *use computer languages to communicate with them*.  
>Programs are written using *programming languages*.
### 机器语言(Machine Language)
- 一系列原始指令
- 二进制代码形式
>Program with native machine language is a *tedious process*. Moreover the programs are highly difficult to read and modify. For example, to add two numbers, you might write an instruction in binary like this: 1101101010011010
### 汇编语言(Assembly Language)
通过编译器将汇编语言转化为二进制代码
![[微信截图_20230904191249.png|500]]
### 高级语言
易懂
## Java语言特性
- Java是鲁棒的程序设计语言
- 易于学习
- 可以独立于平台运行小程序和应用程序
	主要靠Java虚拟机（JVM, Java Virtual Machine)在目标及实现平台无关性
	JVM解释执行
	“一处编译多处执行”
- 面向对象

### 计算机基本组成
![[微信截图_20230904192535.png|500]]
### Interpreting/Compiling Code
>A program written in a high-level language is called a source program or *source code*.
>Because a computer cannot understand a *source program*, a source program must be translated into *machine code* for execution.
>The translation can be done using another programming tool  called an *interpreter* or a *compiler*.
#### 解释器(Interpreter)
**每次从源码解释一条语句(statement)为机器码并立即执行**，一条源码可能被解释成多条机器指令
缺点：比较慢
优点：容易跨平台，只需要对应平台安装对应解释器即可
![[微信截图_20230904195805.png|500]]
#### 编译器(Compiler)
将所有源码一次性转化为机器语言(二进制代码)文件并执行
优点：执行速度快
缺点：依赖具体的机器

解释执行用户得到的是源码，编译执行用户得到的是exe文件
![[微信截图_20230912090719.png|500]]

#### 两种方式比较
![[微信截图_20230912091132.png|625]]

### Java虚拟机结合了编译与解释的长处
![[微信截图_20230912091252.png|500]]
将源码编译成八进制码(class文件)，再在不同平台上解释运行

**在速度与平台兼容上折中：**
- 编译：提高了效率
- 解释：跨平台
- Bytecode：八进制码，机器读不懂，需要解释器

**JVM(Java VIrtual Machine)：**
- Java虚拟机，负责**解释**
- 虚拟机有多个版本，不同平台安装不同虚拟机
- 只要机器安装了对应类型的JVM，这台机器就可以运行Java程序
![[微信截图_20230912091547.png|500]]
### Java语言的特性
#### 可靠性
- 强类型规定
- 摒弃指针
- 自动垃圾收集
- 运行时检查
- 异常处理机制
#### 安全性
- 字节码校验器
- 类装载器
- 访问限制(封装)
![[微信截图_20230912091749.png|500]]
#### 多线程
- Java环境本身就是多线程的
- Java语言内置多线程控制，可大大简化多线程应用程序开发
- Java的多线程支持在一定程度上受到运行时支持平台的限制
#### 自动垃圾回收
![[微信截图_20230912092022.png|500]]
#### 总结
![[微信截图_20230912092302.png|500]]
### Java开发环境
#### JDK
- javac：*Java编译器*，将源代码转换为字节代码，生成*class文件*
- java：*Java解释器*，直接从类文件*执行*Java应用程序代码
- javadoc：根据Java源代码及其说明语句生成的HTML文档
- javah：产生可以调用Java过程的C过程，或建立能被Java程序调用的C过程的头文件
#### Java集成开发环境(IDE)
![[微信截图_20230912094303.png|500]]
#### JVM,JRE和JDK
![[微信截图_20230912094349.png|500]]

**第一个文件: Hello World:**
```java
public class HelloWorld{
	public static void main(String args[]){
		System.out.println("Hello World.");
	}
}
```

cmd窗口指令：
cd：改变当前目录，一个点到上一级; cd. ：当前目录；cd.. ：切换到副目录
dir：显示目录文件
DOS：磁盘操作系统

配置路径：
- 找到JDK安装位置
- 打开环境变量
- 添加JAVA_HOME=C:\\...
- path添加%JAVA_HOME%\\bin
- 添加CLASSPATH=.;%JAVA_HOME%\\lib;%JAVA_HOME%\\lib\\dt.jar;%JAVA_HOME%\\lib\\tools.jar

### 另一种方法：使用eclipse执行
#### eclipse的配置
- 将文件夹设置为原本设好的javacode文件夹中
- new java program - hello
- new class - HelloWorld ， 选择`public static void main(String [] args)`，相当于`main`函数

#### 创建两个class执行
第一个：Greeting:
```java
package hello;

public class Greeting{
	private String sal;
	Greeting(String s){
		sal = s;
	}
	public void greet(String whom){
		System.out.println(sal+" "+whom);
	}
}
```
第二个：TestGreeting:
```java
package hello;

public class TestGreeting{

	public static void main(String[] args){
	Greeting hello = new Greeting ("Hello");
	hello.greet("World");
	}
}
```

### Java代码编写
- Java区分大小写，各种括号成对出现
- *常量命名全部大写*，单词间用下划线隔开，力求语义表达完整
- *类名、变量名一般用单词首字母大写的混合编写方法*
- 注释符号"//"，对程序无影响
- 每个source file **最多一个public class**，文件名为公开类名；多个类可以放在一起，只要只有一个public class就可以
### Java程序编译与动行
例如：
```java
package hello;

public class TestGreeting{

	public static void main(String[] args){
	Greeting hello = new Greeting ("Hello");
	hello.greet("World");
	}
}

class Greeting{
	private String sal;
	Greeting(String s){
		sal = s;
	}
	public void greet(String whom){
		System.out.println(sal+" "+whom);
	}
}
```
- 主类中的`main`函数格式固定，是程序的入口点
	`main(String[] args) / main(String args[])`
- javac编译后在同目录下生成同名的.class文件
![[微信截图_20230912094659.png|500]]
### Java源文件结构
![[微信截图_20230912095212.png|500]]
#### Package语句
![[微信截图_20230912095300.png|500]]
![[微信截图_20230912095441.png|500]]
#### import语句
![[微信截图_20230912095541.png|500]]
### Java语言元素
- **标识符**
	- 标识符是用于标识Java程序中的类、方法、变量等元素的名称。
	- 以字母、\_\_、$ 开始作为首字符
	- 其他字符可以是上面三种字符和数字
	- Java关键字不是标识符，但可以作为其中的一部分
	- 标识符大小写相关，且无长度限制
	![[微信截图_20230912100120.png|550]]
- 关键字
	- Java语言本身使用的标识符![[微信截图_20230912100232.png|500]]![[微信截图_20230912100309.png|500]]
- 数据类型
- 运算符
- 分隔符
	- 用来确认代码在何处分割
	- 如"` `"`,`' `;` "`:`' 都是Java语言分隔符
### 补充
![[微信截图_20230912100505.png|500]]
![[微信截图_20230913202352.png|500]]

关键字（Keywords）在Java中具有特殊含义，与标识符和变量名有显著的区别：

1. **关键字（Keywords）：**
   - 关键字是Java语言预定义的、具有特定意义的单词。每个关键字在Java语言中都有固定的、预先定义的含义，且这个含义不能被改变。
   - 关键字用于执行特定的功能和任务，如定义数据类型、控制程序流程等。例如，`if`、`class`、`public`、`static`、`void`、`new` 等。
   - 关键字不能被用作标识符或变量名。也就是说，你不能将一个关键字用作类名、方法名或变量名。

2. **标识符：**
   - 标识符是程序员定义的名字，用于标识变量、方法、类、接口等。它们遵循特定的命名规则，但可以自由地由程序员选择。
   - 标识符可以是任何合法的名称，只要它不是Java的关键字，并且遵循命名规则。

3. **变量名：**
   - 变量名是标识符的一种，用于标识存储数据的内存位置。变量名的命名也必须遵守Java的标识符命名规则，并且不能使用关键字。

**区别：**
   - **预定义与自定义：** 关键字是Java语言预定义的，具有特定的含义和用途，不能更改；而标识符和变量名是由程序员自定义的，可以自由命名。
   - **用途：** 关键字用于指定Java语言的结构和控制程序流程；标识符用于为变量、方法、类等提供自定义名称。
   - **命名限制：** 关键字是固定的单词，不能用作标识符或变量名；标识符和变量名有较大的自由度，但需要遵守命名规则且不得使用关键字。
