注意：系统导论里面的汇编代码格式是ATT格式，而计组是Intel格式，注意二者区分
# 基础
体系结构(又称ISA：指令集体系结构) 要进行处理器设计，需要理解或者写汇编/机器代码，ISA定义了处理器状态、指令的格式，以及每条指令对状态的影响

![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611100955.png)
程序员可见的状态Programmer-Visible State
▪ PC程序计数器PC: Program counter
	▪ **下条指令地址**Address of next instruction
	▪ 称为RIP Called “RIP” (x86-64)
▪ 寄存器堆Register file
	▪ 程序**频繁使用的数据**Heavily used program data
▪ 条件码Condition codes
	▪ 存储有关最近的**算术和逻辑运算的状态信息**
	Store status information about most recent arithmetic or logical operation
	▪ 用于**条件分支**Used for conditional branching
▪内存Memory
	▪ 字节**可寻址的数组**Byte addressable array
	▪ **代码和用户数据**Code and user data
	▪ 栈用于**支持过程、函数**Stack to support procedures

```lisp
addq %rbx, %rax
```
相当于 rax += rbx
这些是64位寄存器，因此我们认为这是64位加法

## 访问信息
x86-64 寄存器
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611101940.png)
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611102006.png)
可以通过不同的指令访问不同字节，如16位访问最低2个字节，32位访问最低的4个字节，64位访问全部字节

操作：
- 传送数据
	- 从内存装载数据到寄存器
	- 存储寄存器数据到内存
- 算术运算
	- 对寄存器或内存数据执行算术运算
- 传递控制
	- 无条件跳转到/从过程
	- 条件分支
	- 间接分支
## 传送数据
```lisp
movq Source, Dest
```
将Source传送到Dest中，为64位数据

### 操作数类型
- 立即数：**常量整数**
	▪ 例如：Example: $0x400, $-533
	▪ 类似C常量，\$前缀
	▪ 编码1，2或4字节
- 寄存器：16个**整数寄存器之一**
	▪ 例如：Example: %rax, %r13
	▪ 但%rsp保留有特殊用处
	▪ 其它对特定指令有特殊用途
- 内存：因为是movq，所以是8个连续字节，需要给出内存地址
	- 最简单的例子：Simplest example: (%rax)
	- 各种其它“寻址方式”

movq操作数组合：
![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611102848.png)
注意，**单条指令中不能进行内存到内存的传送**
### 内存寻址方式
![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611103053.png)

在函数调用中，第一个参数存放在rdi寄存器，第二个在rsi寄存器
- 简单的swap函数，由于不能直接进行内存到内存的传送，因此要通过寄存器执行
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611103234.png)
数据先传送到rax和rdx寄存器中：
![image.png|450](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611103344.png)
随后再传送回对应的内存位置：
![image.png|475](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611103455.png)
### 复杂地址的寻址
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611103911.png)
D: 立即数偏移
Ri: 索引寄存器，表示第几个元素
S: 比例因子，1，2，4，8作为字节数
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611104045.png)

### 数据传送指令类型
![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611105441.png)
movl 以寄存器作为目的时，会把该寄存器的高位4字节设置为0，原因是x86惯例，任何为寄存器生成32位值得指令都会把寄存器高位置为0
![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611105721.png)

### 零扩展和符号扩展
将较小的源值复制到较大的目的时使用
movz将目的中剩余的字节填充为0，movs将通过符号填充
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611105958.png)
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611110019.png)
注意一个cltq，这个指令不需要source和dest，直接填充，且为符号填充

这个例子很好显示出了mov, movz, movs区别：
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611110241.png)

## 算数与逻辑运算
### leaq 指令
```lisp
leaq Scr, Dst
```
Src是寻址方式表达式
设置Dst成为表达式指示的地址

- 用法
	- 计算地址不访问内存，`p=&x[i]`
	- 计算x + k \*i
![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611111502.png)
leaq只是一个根据公式进行计算的指令，内部内容不一定需要是地址
>[!queation]为什么倾向使用lea?
>CPU设计师倾向使用：计算一个对象的指针
>例如：数组元素![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612155949.png)
>编译器设计人员喜欢用它实现普通计算
>- 可以在一条指令中做复杂的计算
>- x86仅有的几个三操作数指令之一
>- 并不影响条件码（我们后面再讨论）![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612160101.png)

### 算数运算
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611112302.png)
移位：左移sal, shl，算数右移sar, 逻辑右移shr
注意移位要么存在cl，要么用立即数
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611112402.png)
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611112806.png)
#### 特殊的算数运算
注意imulq，有两种算术运算，如果单操作数，对R[rax]内容乘S(补码乘法)，再把内容分配到两个寄存器中，其中rdx存放高64位，rax存放低64位
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240611115649.png)
cqto隐含操作数，效果是将rax内容转化为8个字，进行符号扩展后分到rdx和rax寄存器
对于除法，商存在rax，而余数存在rdx
# 控制
C语言的控制流：
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612160231.png)
汇编语言：
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612161218.png)

### 处理器状态
![image.png|625](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612161340.png)
## 条件码
**单个比特位寄存器**
设置时与有无符号无关，只看二进制位的情况即可
- CF 进位标志Carry Flag (对无符号数for unsigned)
- SF 符号标志Sign Flag (对有符号数for signed)
- ZF 零标志Zero Flag
- OF 溢出标志Overflow Flag (对有符号数for signed)

条件码是**隐式设置**的，通过算数或者逻辑运算进行设置
```lisp
addq %edx, %eax
```
- set CF 如果从最高有效位进位（无符号溢出）
- set ZF 如果结果为零
- set SF 如果结果小于零（有符号数）
- set OF 如果补码（有符号数）溢出

**leaq指令不设置条件码**
![image.png|475](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612162425.png)
![image.png|475](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612162434.png)
![image.png|475](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612162442.png)
![image.png|475](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612162452.png)

### 设置条件码的指令
#### cmp
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612162716.png)
#### test
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612162740.png)
test两个相同的寄存器可以用来检查寄存器值为0，负数还是正数
### 读取条件码 SetX指令
- 根据条件码组合设置目的操作数**低字节**成0或1
- **不要改变**剩余的7个字节
![image.png|600](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612163559.png)
#### setl
![image.png|575](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612170205.png)
这里对最后一种情况解释下：当SF为1的时候，说明最高位为1，但是同时出现了overflow溢出，说明要么上溢，要么下溢，但是下溢出不应该出现最高位为1，所以应该为上溢，又因为这里是减法，说明是减去了一个负数，而且结果还超了，那么a应该为一个正数，那b是负数，说明没有小于

设置了以后通常用movzbl(零扩展从字节到双字)来完成工作
![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612171032.png)

> [!note] 
> **setl 和movzbl**不会改变条件码
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612171802.png)
## 使用控制的条件分支
跳转指令jx：
jmp为无条件跳转
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612171904.png)

示例：
在函数中，返回值存储在%rax，第一个参数%rdi, 第二个参数在%rsi
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612171937.png)
为了更好提前汇编的分支，这里用goto表达：
```c
long absdiff_j
(long x, long y)
{
	long result;
	int ntest = x <= y;
	if (ntest) goto Else;
	result = x-y;
	goto Done;
	
	Else:
	result = y-x;
	
	Done:
	return result;
}
```
抽象结果：把测试条件反过来，如果没有满足测试，那么进入else，反之直接进入done

通常的代码格式：![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612172312.png)
## 使用条件传送指令实现条件分支
```c
val = Test ? Then_Expr : Else_Expr;
```
goto版本：
```c
result = Then_Expr;
eval = Else_Expr;
// 先让结果为then的结果，再通过判断是否要让result为else
nt = !Test;

if(nt)
	result = eval;

return result;
```
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612173424.png)
使用条件传送：
```c
/*对原本绝对值作差函数的改写*/
long cmovdiff(long x, long y){
	long rval = y - x;  // 计算出了两个作差值，看结果是什么就用哪个
	long eval = x - y;
	long ntest = x >= y;

	if(ntest) rval = eval;
	return rval;
}
```
对应的汇编代码：
注意一开始x - y 是放在rax寄存器的，这样可以少用一个，同时后面传送的时候直接覆盖就好
```lisp
absdiff:
	movq %rdi, %rax  # x
	subq %rsi, %rax  # result = x-y 放在rax
	movq %rsi, %rdx
	subq %rdi, %rdx  # eval = y-x 放在rdx
	cmpq %rsi, %rdi  # x-y
	cmovle %rdx, %rax  
	# if <=, result = eval 如果rdi小于rsi，说明结果是y>x那么就把rdx的值给到rax
	ret
```

优点：有利于CPU流水线运行
缺点：
	- 需要大量计算
		- 同时算了两个值，而条件控制只需要计算一个值![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612174928.png)
	- 计算存在风险
		- 当遇到空指针的时候可能会有空指针引用![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612174912.png)
	- 计算可能有副作用
		- 因为同时算了两个值，如果两个值对一个变量进行了改变，那么可能会导致计算出现问题![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612174852.png)
## 循环
### do-while
![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612185407.png)
这里的jne实际上是看的是shrq的结果，因为移位运算也会修改条件码
### while
由于while在第一次执行body-statement之前，会对test-expr求值，可能会在这个阶段就终止循环，因此，需要使用特别的方法：跳转到中间和guarded-to
#### 跳转到中间
![image.png|525](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612185523.png)
跳转到中间就是第一次先跳转到test，经过条件判断后再跳转回到loop
#### do-while转换
先判断，然后后面的跟do-while一样
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612190152.png)
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612190314.png)
### For
```c
#define WSIZE 8*sizeof(int)
long pcount_for
(unsigned long x)
{
	size_t i;
	long result = 0;
	for (i = 0; i < WSIZE; i++)
	{
		unsigned bit =
		(x >> i) & 0x1;
		result += bit;
	}
	return result;
}
```
对于for循环，实际上可以将其优化为while循环：
![image.png|475](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612190725.png)
```c
long pcount_for_while
(unsigned long x)
{
	size_t i;
	long result = 0;
	i = 0;
	while (i < WSIZE)
	{
		unsigned bit =
		(x >> i) & 0x1;
		result += bit;
		i++;
	}
	return result;
}
```
## switch语句
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612190953.png)
switch语句有特殊的跳转表结构：
跳转表是一个数组，表项i是一个代码段的地址，实现当开关索引值等于i时程序采用的动作。
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612191558.png)
跳转表：
- 最开始是L4
- 对于多情况标签，将指向同一个L字段
- 对于default的所有缺失情况，也将指向同一个L字段
- 对于落入其他情况标签，
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612192112.png)
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612192233.png)
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612192415.png)
注意，w不一定需要在外面初始化，因为有的跳转可能不需要w，但是在内部设置w的时候注意可能会把w的计算写在两个L字段中：
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612192831.png)

![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240612192849.png)
