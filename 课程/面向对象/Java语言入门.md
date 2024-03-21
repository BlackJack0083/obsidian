# Java语言入门
![[微信截图_20230916152959.png]]
### 计算机中的数据存储
原码：
	byte: -127~+127
补码：
	数据范围：-128~127
	正数：补码 = 原码
	负数：符号位不变，其余取反，最后+1
		$2^8 - |x|$
	正数+负数 <=> 正数+负数的补码
### Java语言的元素——数据类型
#### 8种基本类型
- `char`(只能单引号)(16)
	- **字符串**数据取值范围为`0-65535`，或者说\\u0000 - \\uFFFF
	- `String`类型表示一串字符，**不是8种基本类型**，使用**双引号**括起来，最后字符不是`\0`
	
- `numeric`: `byte`(8), `int`(32), `short`(16), `long`(64)
	*eg. 2L(long 类型), 077L(8进制), 0xBABEL(16进制)*
	将一个int型变量赋值为short型或byte型变量，必须**显式使用类型转换**
	```java
	short b1 = 1, b2 = 2;
	// short b3 = b1 + b2; // 会报错，因为b1+b2会变成int类型，需要进行类型转换
	short b3 =(short)(b1 + b2); 

	byte b1 = 1, b2 = 0;
	byte b3 = (byte)(b1 + b2); // 同理

	long l1 = 65536 * 63356; // 乘法运算越界，l3为0
	long l4 = 65535L - 63356; // l4 = 4294967296L
```
	以最长的数字类型决定类型

**`char`类型可以和`short`类型互相转化**
```java
short s = 100;
char c = 'A';

// short 到 char 的隐式转换
c = s;

// char 到 short 的隐式转换
s = c;
```
- `float`: float(32)(不能超过$3.4 * 10^{31}$, double(64)![[微信截图_20230916153326.png|525]]
- *boolean*: true / false，false为缺省值
	- eg. boolean b = (b1 && b2) != false
- 浮点数*缺省为double型*，整数*缺省为int型*

Java中的基本类型转换分为两类：自动类型转换（隐式转换）和强制类型转换（显式转换）。
### 自动类型转换（隐式转换）
自动类型转换发生在当赋值给目标类型时，源类型的数据范围小于或等于目标类型时。Java编译器会自动完成这种转换。常见的自动类型转换包括：
- **从低精度到高精度：** 
  - `byte` 到 `short`、`int`、`long`、`float` 或 `double`
  - `short` 到 `int`、`long`、`float` 或 `double`
  - `char` 到 `int`、`long`、`float` 或 `double`
  - `int` 到 `long`、`float` 或 `double`
  - `long` 到 `float` 或 `double`
  - `float` 到 `double`
### 强制类型转换（显式转换）
强制类型转换是在赋值时，源类型的数据范围大于目标类型，或者是两种不兼容类型之间的转换。在这种情况下，需要显式地进行类型转换。强制类型转换可能导致数据丢失或精度降低。语法是在要转换的值前面加上目标类型，括在圆括号内。
例如：
```java
double d = 9.7;
int i = (int) d; // 强制将double类型转换为int类型
```
这里，`double` 类型的 `d` 被强制转换为 `int` 类型的 `i`。由于 `double` 类型的精度高于 `int`，因此可能会丢失小数部分。
### 注意事项
- 当进行强制类型转换时，应当意识到可能的数据丢失或精度降低，特别是从高精度到低精度的转换。
- 对于布尔类型，没有隐式或显式的转换到或从其他类型。
- 在使用强制类型转换时，应当小心，确保转换是符合预期的。

#### 其余均为对象
`class`,`array`, 引用   
### Java运算符
此处单独把位运算符摘出，以作为提醒
![[微信截图_20230916162723.png|500]]
### Java语言中的变量与常量
![[微信截图_20230916154730.png|500]]

**变量和作用区域：**
![[微信截图_20230916162906.png|500]]
```java
public class TestScope {
	public static void main(String[] args) {
		ScopeExample scope = new ScopeExample();
		scope.firstMethod();
		System.out.println(scope.getValue()); // 打印15
	}
}

class ScopeExample{
	private int i = 1;
	public void firstMethod(){
		int i = 4, j = 5; this.i = i + j;
		secondMethod(7);
	}
	public void secondMethod(int i){
		int j = 8; this.i = i + j;
	}
	public int getValue() {
		return this.i;
	}
}
```
在这个例子中，`scope`的`i` 一开始先被初始化为1，即`this.i = 1`，而后再调用`firstMethod`方法，使得`i`变成9，再调用`secondMethod`，使`i`变成15，因为这里始终修改的是`this.i`，是成员变量，所以可以在`getValue()`方法中显示
#### 引用类型变量
![[微信截图_20230916170342.png|550]]![[微信截图_20230916170354.png|550]]
**此处必须要注意：对象类型的变量里面存储的只是对象的地址!!**
[[方法重载与静态成员#判断相等]]
成员变量作为引用类型会自动初始化为`null`，但局部变量需要自行初始化（**数组除外**）

### Java语句
由于大部分与C语言一致，故不赘述，此处只对特殊的语句形式进行叙述
![[微信截图_20230916170710.png|500]]![[微信截图_20230916170718.png|500]]
### 方法调用中的参数值传递
![[微信截图_20230916171000.png|500]]![[微信截图_20230916171009.png|500]]
在 Java 中，确实不支持传递变量的地址（引用）进行直接的地址传递，因此不能像一些其他语言（如 C++）那样通过操作地址来实现 swap。在 Java 中，传递的是值的拷贝，而不是变量的地址。

然而，你可以通过其他方式实现 swap，其中一种方式是通过使用一个包装类或数组来传递参数。下面是两种可能的实现方式：

### 使用一个包装类
```java
class IntWrapper {
    int value;

    IntWrapper(int value) {
        this.value = value;
    }
}

public class SwapExample {
    public static void swap(IntWrapper a, IntWrapper b) {
        int temp = a.value;
        a.value = b.value;
        b.value = temp;
    }

    public static void main(String[] args) {
        IntWrapper x = new IntWrapper(5);
        IntWrapper y = new IntWrapper(10);

        swap(x, y);

        System.out.println("After swap: x = " + x.value + ", y = " + y.value);
    }
}
```

### 使用数组
```java
public class SwapExample {
    public static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    public static void main(String[] args) {
        int[] array = {5, 10};

        swap(array, 0, 1);

        System.out.println("After swap: array[0] = " + array[0] + ", array[1] = " + array[1]);
    }
}
```

这两种方法都是通过传递一个包装类对象或数组，从而达到在方法内交换值的目的。这是因为对象和数组在 Java 中是引用类型，而引用是按值传递的，因此你可以修改对象或数组内的值。
### Java数组
**数组是一个对象！！**
数组定义仅仅分配一个用于访问数组的**引用**，真正的数组元素空间在使用`new`语句或初始化数组时创建
#### 创建数组
![[微信截图_20230916171147.png|500]]![[微信截图_20230916171244.png|500]]![[微信截图_20231230213346.png|500]]
#### 数组元素
数组元素也要重新`new`一个
![[微信截图_20230916171435.png|500]]
`int` 数组会自动初始化为0，对其他的数组则是自动初始化为`null`
![[微信截图_20231230213933.png|500]]
#### 多维数组
![[微信截图_20230916171521.png|500]]
在Java中，可以创建不规则的二维数组，也就是每行长度不同的数组。要遍历这样的数组，你需要使用嵌套循环：外层循环遍历行，内层循环遍历每行的列。由于每行的长度可能不同，因此内层循环应该依赖于当前行的长度。

下面是遍历不规则二维数组的一个例子：
```java
public class IrregularArrayExample {
    public static void main(String[] args) {
        // 创建一个不规则的二维数组
        int[][] irregularArray = {
            {1, 2, 3},
            {4, 5},
            {6, 7, 8, 9}
        };

        // 遍历不规则数组
        for (int i = 0; i < irregularArray.length; i++) {
            for (int j = 0; j < irregularArray[i].length; j++) {
                System.out.print(irregularArray[i][j] + " ");
            }
            System.out.println(); // 换行，以分隔不同的行
        }
    }
}
```

在这个例子中，`irregularArray` 是一个不规则的二维数组。外层循环（`for (int i = 0; i < irregularArray.length; i++)`）遍历数组的每一行，而内层循环（`for (int j = 0; j < irregularArray[i].length; j++)`）遍历当前行中的每一个元素。使用 `irregularArray[i].length` 确保我们正确地获取每一行的长度。

#### 数组拷贝
注意拷贝基本类型和引用类型数组的区别
![[微信截图_20231230214826.png|500]]
![[微信截图_20231230214942.png|500]]

补充：![[微信截图_20231231215310.png|500]]![[微信截图_20231231215638.png|500]]
![[微信截图_20231231220014.png|500]]

![[微信截图_20240101172553.png|500]]
在Java中，不同数据类型占用的内存大小通常如下（但具体大小可能依赖于JVM的实现和运行的平台）：
- `int` 类型通常占用 4 个字节。
- `char` 类型占用 2 个字节。

因此，对于 `Sample` 类的一个实例：
- `int a` 将占用 4 个字节。
- `char c` 将占用 2 个字节。
- 共6个字节

![[微信截图_20240101173021.png|500]]
在Java中，`char` 类型是一个16位的Unicode字符。虽然 `char` 类型通常用于表示字符，但它也可以用来存储整数值，因为它是一个整数数据类型。`char` 类型的范围是 0 到 65535，可以存储 Unicode 字符的编码值。

在你的例子中，`char c = 33;` 是合法的，因为整数 `33` 在 `char` 的范围内。在ASCII表中，`33` 对应着感叹号字符 `'!'` 的编码。

这是因为在Java中，可以将整数直接赋给 `char` 类型，而无需显式强制类型转换。当整数值在 `char` 类型的范围内时，它将被隐式转换为对应的字符。

要注意的是，如果整数值超出 `char` 类型的范围，你可能需要进行显式的类型转换。例如，`char c = (char) 1000;`，这里使用了强制类型转换，因为 `1000` 超出了 `char` 类型的范围。