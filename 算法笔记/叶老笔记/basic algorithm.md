# 前置知识

## 递归

> To iterate is human, to recurse, divine.—— L. Peter Deutsch

这句话是“神童”L. Peter Deutsch的名言，中文意思大概就是“迭代是人，递归是神！”

简单递归：

> 从前有座山，山上有座庙，庙里有个老和尚，有一天老和尚给小和尚讲故事，说从前有座山，山上有座庙，庙里有个老和尚，有一天老和尚给小和尚讲故事，说从前有座山，山上有座庙，庙里有个老和尚，有一天老和尚给小和尚讲故事，说从前有座山，山上有座庙......

在数学与计算机科学中，递归(Recursion)是指在函数的定义中调用函数自身的方法。实际上，递归，顾名思义，其包含了两个意思：**递** 和 **归**，这正是递归思想的精华所在。

递归是一个奇妙的思维方式，通常把一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解，一句话就是：把大事化小。递归可以用极简的代码解决一些复杂的问题，但要想真正领悟递归的精髓、灵活地运用递归思想来解决问题却并不是一件容易的事情。

### 经典案例

### 阶乘

【**问题描述**】 计算阶乘。阶乘是基斯顿·卡曼（Christian Kramp，1760～1826）于 1808 年发明的运算符号。 一个正整数的阶乘（factorial）是所有小于及等于该数的正整数的积，并且 $0$ 的阶乘为 $1$。自然数 $n$ 的阶乘写作 $n!$，即 $f(n) = n! = n f(n-1)$，$f(0)=f(1)=1$。

【**思路**】按照“大事化小”的思路，如果我们能够计算出 $f(n-1)$，那么 $f(n) = n \cdot f(n-1)$；如果我们能够计算出 $f(n-2)$，那么 $f(n-1) = (n-1) \cdot f(n-2)$；一致 **递** 下去......那么到何时何处 **归** 呢？显然不能无休止地不断传递下去。对于这个问题，当 $n=1$ 时，根据定义可以知道，$f(1)=2$，这时候就应该开始 **归** 了，也就是 $f(2) = 2 \cdot f(1) = 2$；$f(3) = 3 \cdot f(2) = 6$；.......；$f(n) = n \cdot f(n-1)$。

实现代码：

```c++
//求n的阶乘
long long factorial(int n)
{
    if (n == 0 || n == 1)
        return 1;                      //递归出口
    else
        return n * factorial(n - 1);  // 递归调用
}
```

**递**

- 如果我们调用 `factorial(5)`，当进入 `factorial()` 函数体后，由于形参 `n` 的值为 `5`，不等于 `0` 或 `1`，所以执行 `n * factorial(n-1)`，也即执行 `5 * factorial(4)`。为了求得这个表达式的结果，必须先调用 `factorial(4)`，并暂停其他操作。换句话说，在得到 `factorial(4)` 的结果之前，不能进行其他操作。这就是第一次递归。
- 调用 `factorial(4)` 时，实参为 `4`，形参 `n` 也为 `4`，不等于 `0` 或 `1`，会继续执行 `n * factorial(n-1)`，也即执行 `4 * factorial(3)`。为了求得这个表达式的结果，又必须先调用 `factorial(3)`。这就是第二次递归。
- 以此类推，进行四次递归调用后，实参的值为 `1`，会调用 `factorial(1)`。此时能够直接得到常量 `1` 的值，并把结果 `return`，就不需要再次调用 `factorial()` 函数了，当前递归就结束。

**归**

递归进入到最内层的时候，递归就结束了，就开始逐层退出了，也就是 逐层执行 return 语句。

- `n` 的值为 `1` 时达到最内层，此时 return 出去的结果为 `1`，也即 `factorial(1)` 的调用结果为 1。
- 有了 `factorial(1)` 的结果，就可以返回上一层计算 `2 * factorial(1)` 的值了。此时得到的值为 `2`，`return` 出去的结果也为 `2`，也即 `factorial(2)` 的调用结果为 `2`。
- 以此类推，当得到 `factorial(4)` 的调用结果后，就可以返回最顶层。经计算，`factorial(4)` 的结果为 `24`，那么表达式 `5 * factorial(4)` 的结果为 `120`，此时 `return` 得到的结果也为 `120`，也即 `factorial(5)` 的调用结果为 `120`，这样就得到了 $5!$ 的值。

可以看到计算过程如下：

```c
===> fact(5)
===> 5 * fact(4)
===> 5 * (4 * fact(3))
===> 5 * (4 * (3 * fact(2)))
===> 5 * (4 * (3 * (2 * fact(1))))
===> 5 * (4 * (3 * (2 * 1)))
===> 5 * (4 * (3 * 2))
===> 5 * (4 * 6)
===> 5 * 24
===> 120
```

【**提示**】在设计递归时需要注意几点：

- 分析问题，努力把问题 **大事化小**。
- 找到递归的出口。所有的递归一定要有一个结束的出口，否则容易死循环。因为 **大事化小**，所以处理问题的规模会越来越小，小到容易处理的时候就不需要再化了，直接处理。

### 进制转换

【**问题描述**】给定一个正整数 $n$，打印其对应的二进制序列。

【**思路**】容易知道 $n \% 2$ 可以得到最后一个二进制，然后 $n = n / 2$；$n \% 2$ 又得到最后一个二进制，以此类推，不断进行。每操作一次，$n$ 的值会减半，所以规模逐渐减小。当 $n < 2$ 时，可以直接打印，这个就是递归的出口。

```c++
void print(int n) {
    if(n < 2) {
        printf("%d", n);
        return;            // 递归出口
    }
  
    print(n/2);            // 递归调用
    printf("%d", n % 2);   // 每一步骤打印一位
}
```

调用 `print(10)`，计算过程如下：

```c
===> print(10)
===> print(10/2) --> print(5) |中断| printf("%d", 10 % 2);
===> print(5/2)  --> print(2) |中断| printf("%d", 5  % 2);
===> print(2/2)  --> print(1) |中断| printf("%d", 2  % 2);
===> print(1)    --> printf("%d", 1);return;
===> print(2)  |恢复| printf("%d", 2  % 2);
===> print(5)  |恢复| printf("%d", 5  % 2);
===> print(10) |恢复| printf("%d", 10 % 2);
```

【**思考**】

1. 这里为什么不用 $n == 0$ 作为结束，而是用 $n < 2$?
2. 如果需要固定位数的二进制，例如转换为 $32$ 位二进制，如何修改？

### 汉诺塔

【**问题描述**】法国数学家爱德华·卢卡斯曾编写过一个印度的古老传说：在世界中心贝拿勒斯（在印度北部）的圣庙里，一块黄铜板上插着三根宝石针。印度教的主神梵天在创造世界的时候，在其中一根针上从下到上地穿好了由大到小的 $64$ 片金片，这就是所谓的汉诺塔。不论白天黑夜，总有一个僧侣在按照下面的法则移动这些金片：一次只移动一片，不管在哪根针上，小片必须在大片上面。僧侣们预言，当所有的金片都从梵天穿好的那根针上移到另外一根针上时，世界就将在一声霹雳中消灭，而梵塔、庙宇和众生也都将同归于尽。

![](https://www.dotcpp.com/oj/ueditor/php/upload/image/20221108/1667877612283493.png)

【**思路**】现在的问题是 **将 $n$ 块圆盘从A柱通过B柱搬到C柱**。从正向考虑，这个问题有点复杂，但是反过来思考，如果我们能够将上面的 $n-1$ 块圆盘从A柱通过C柱搬到B柱，那么就可以直接将最底下的第 $n$ 块搬到C柱，然后再将 $n-1$ 块圆盘从B柱通过A柱搬到C柱，任务完成。现在将一个问题转换为两个子问题 **将 $n-1$ 块圆盘从A柱通过C柱搬到B柱** 和 **将 $n-1$ 块圆盘从B柱通过A柱搬到C柱**。虽然问题多了，但是问题规模减小了，如此进行下去，问题肯定可以解决。

```c++
void hanoi(int n, char a, char b, char c)
{
    if(n == 0)
         return;
    else {
        hanoi(n-1, a, c, b);
        printf("move %d from  %c to  %c\n", n, a, c);
        hanoi(n-1, b, a, c);
    }
}
```

假设 $n$ 块圆盘需要移动的次数为 $T(n)$，那么 $T(n) = 2T(n-1) + 1$，求解非齐次差分方程容易得到 $T(n) = 2^n-1$。当 $n=64$ 时，$T(64) = 2^{64}-1 = 18446744073709551615$。假如每秒钟一次，需要5845.42亿年以上。

【**扩展**】前面说过迭代是人，递归是神，但对于汉诺塔问题，则恰好相反。想要写出汉诺塔的非递归形式并不容易，可以借助 **格雷码** 来处理。有兴趣的可以参考其他资料。

### 斐波那契数列

递归有时候可以用极简的代码解决复杂问题，但是有时候递归并不总是那么优雅。下面的斐波那契数列就是一个不那么优雅的例子。

【**问题描述**】斐波那契数列（Fibonacci sequence），又称黄金分割数列，因数学家列昂纳多·斐波那契（Leonardoda Fibonacci）以兔子繁殖为例子而引入，故又称为“兔子数列”，指的是这样一个数列：$1、1、2、3、5、8、13、21、34、……$在数学上，斐波那契数列以如下被以递推的方法定义：$F(0)=1，F(1)=1, F(n)=F(n – 1)+F(n – 2)(n \ge 2，n \in \mathbb{N})$。

【**解题思路**】根据递推式，我们可以很容易写出斐波那契数列的递归代码。

```c
long long fib(int n) {
    if(n == 0 || n == 1)
        return 1;
    return fib(n-1) + fib(n-2);
}
```

可以尝试运行一下，当 $n = 40$ 时，需要的时间可能超过20秒。上面的递归形式，主要会重复计算很多。例如调用 `fib(5)`时，
![](https://pic2.zhimg.com/v2-d1f72b1a9e61ac3e42751d0beb2641d1_r.jpg)
这是一棵二叉树的形式，递归的时间复杂度是 $O(2^n)$。为了避免重复计算，可以采用记忆化的方法。所谓记忆化，就是用一个数组保存中间计算的结果，避免重复计算。

```c
long long f[n] = {0};
long long fib(int n) {
    if(n == 0 || n == 1)
        return f[n] = 1;
    if(f[n])
        return f[n];           // 如果 f[n] 已经计算过，直接返回
  
    return f[n] = fib(n-1) + fib(n-2);
}
```

### 递归的其他例子

#### **最大公约数**

```c
int gcd(int a, int b)
{
   return b ? gcd(b, a % b) : a;
}
```

#### **进制转换(36进制以内的转换)**

```c
const char digit[] = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
void print(int n, int k) {
    if(n < k) {
        printf("%c", digit[n]);
        return;
    }

    print(n / k, k);
    printf("%c", digit[n % k]);
}
```

#### **归并排序**

归并排序（英语：Merge sort，或mergesort），是创建在归并操作上的一种有效的排序算法，效率为 $O(n\log n)$。1945年由约翰·冯·诺伊曼首次提出。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用，且各层分治递归可以同时进行。主要两个步骤：

- 分割：递归将数组二分
- 合并：合并两个有序数组

```c
int tmp[N]; // 辅助数组
void merge_sort(int a[], int l, int r)
{
    if (l >= r) return;   // 递归出口

    int mid = l + r >> 1; // 二分
    merge_sort(a, l, mid);  // 左边排序
    merge_sort(a, mid + 1, r); // 右边排序

  
    // 合并 左[l, mid] 和 右[mid+1, r]两边有序数组
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (a[i] <= a[j]) tmp[k++] = a[i++];
        else              tmp[k++] = a[j++];

    while (i <= mid)      tmp[k++] = a[i++];
    while (j <= r)        tmp[k++] = a[j++];

    // 辅助数组复制到原数组
    k = 0;
    while(l <= r) a[l++] = tmp[k++];
}
```

#### **快速排序**

排序算法的思想非常简单，在待排序的数列中，首先要找一个数字作为基准数（pivot）。基准数一般有四种选择：第一个、最后一个、中间一个或者随机选一个。接下来我们需要把这个待排序的数列中小于基准数的元素移动到待排序的数列的左边，把大于基准数的元素移动到待排序的数列的右边。这时，左右两个分区的元素就相对有序了；接着把两个分区的元素分别按照上面两种方法继续对每个分区找出基准数，然后移动，直到各个分区只有一个数时为止。

```c
void quick_sort(int a[], int l, int r) 
{
    if (l >= r) return; // 递归出口

    int pivot = a[l + r >> 1], i = l, j = r; //以中点作为参考点

    //划分为左右两半
    while (i <= j) {
        while(a[i] < pivot) i++; // a[i] 不需要移动
        while(a[j] > pivot) j--; // a[j] 不需要移动
        if (i <= j) 
            swap(a[i++], a[j--]); // a[i] > pivot, a[j] < pivot, 左右交互
    }

    quick_sort(a, l, j); // 左边排序
    quick_sort(a, i, r); // 右边排序
}
```

## 位运算

位运算就是基于整数的二进制表示进行的运算。由于计算机内部就是以二进制来存储数据，位运算是相当快的。

基本的位运算共 $6$ 种，分别为按位与、按位或、按位异或、按位取反、左移和右移。

为了方便叙述，下文中省略「按位」。

【**与、或、异或**】

与、或、异或都是将两个整数作为二进制数，对二进制表示中的每一位逐一运算。

| 运算 | 运算符 | 数学符号表示                   |                 解释                  |
| ---- | :----: | ------------------------------ | :-----------------------------------: |
| 与   |  `&`   | $\&$、$\operatorname{and}$     |   只有两个对应位都为 $1$ 时才为 $1$   |
| 或   |  `\|`   | $\mid$、$\operatorname{or}$    | 只要两个对应位中有一个 $1$ 时就为 $1$ |
| 异或 |  `^`   | $\oplus$、$\operatorname{xor}$ |     只有两个对应位不同时才为 $1$      |

注意区分逻辑与（对应的数学符号为 $\land$）和按位与、逻辑或（$\lor$）和按位或的区别。

异或运算的逆运算是它本身，也就是说两次异或同一个数最后结果不变，即 $a \oplus b \oplus b = a$ 。

举例：

$$
\begin{aligned}
5 &=(101)_2\\
6 &=(110)_2\\
5\operatorname\&6 &=(100)_2 =\ 4\\
5\operatorname|6 &=(111)_2 =\ 7\\
5\oplus6 &=(011)_2 =\ 3\\
\end{aligned}
$$

【**取反**】

取反是对一个数 $n$ 进行的位运算，即单目运算。

取反暂无默认的数学符号表示，其对应的运算符为 `~`。它的作用是把 $n$ 的二进制补码中的 $0$ 和 $1$ 全部取反（$0$ 变为 $1$，$1$ 变为 $0$）。有符号整数的符号位在 `~` 运算中同样会取反。

补码：在二进制表示下，正数和 $0$ 的补码为其本身，负数的补码是将其对应正数按位取反后加一。

## 左移和右移

 `n << i` 表示将 $n$ 的二进制表示向左移动 $i$ 位所得的值。

 `n >> i` 表示将 $n$ 的二进制表示向右移动 $i$ 位所得的值。

举例：

$$
\begin{aligned}
11&=(00001011)_2\\
11<<3&=(01011000)_2=88\\
11>>2&=(00000010)_2=2
\end{aligned}
$$

移位运算中如果出现如下情况，则其行为未定义：

1. 右操作数（即移位数）为负值；
2. 右操作数大于等于左操作数的位数；

例如，对于 `int` 类型的变量 `a` ， `a<<-1` 和 `a<<32` 都是未定义的。

对于左移操作，需要确保移位后的结果能被原数的类型容纳，否则行为也是未定义的。对一个负数执行左移操作也未定义。

对于右移操作，右侧多余的位将会被舍弃，而左侧较为复杂：对于无符号数，会在左侧补 $0$；而对于有符号数，则会用最高位的数（其实就是符号位，非负数为 $0$，负数为 $1$）补齐。

## 位运算的应用

位运算一般有三种作用：

1. 高效地进行某些运算，代替其它低效的方式。
2. 表示集合。
3. 题目本来就要求进行位运算。

需要注意的是，用位运算代替其它运算方式（即第一种应用）在很多时候并不能带来太大的优化，反而会使代码变得复杂，使用时需要斟酌。（但像「乘 2 的非负整数次幂」和「除以 2 的非负整数次幂」就最好使用位运算，因为此时使用位运算可以优化复杂度。）

### 有关 2 的幂的应用

由于位运算针对的是变量的二进制位，因此可以推广出许多与 2 的整数次幂有关的应用。

将一个数乘（除） 2 的非负整数次幂：

```cpp
int mulPowerOfTwo(int n, int m) {  // 计算 n*(2^m)
  return n << m;
}
int divPowerOfTwo(int n, int m) {  // 计算 n/(2^m)
  return n >> m;
}
```

我们平常写的除法是向 $0$ 取整，而这里的右移是向下取整（注意这里的区别），即当数大于等于 $0$ 时两种方法等价，当数小于 $0$ 时会有区别，如： `-1 / 2` 的值为 $0$ ，而 `-1 >> 1` 的值为 $-1$ 。

### 取绝对值

在某些机器上，效率比 `n > 0 ? n : -n` 高。

```cpp
int Abs(int n) {
  return (n ^ (n >> 31)) - (n >> 31);
  /* n>>31 取得 n 的符号，若 n 为正数，n>>31 等于 0，若 n 为负数，n>>31 等于 -1
    若 n 为正数 n^0=n, 数不变，若 n 为负数有 n^(-1)
    需要计算 n 和 -1 的补码，然后进行异或运算，
    结果 n 变号并且为 n 的绝对值减 1，再减去 -1 就是绝对值 */
}
```

### 取两个数的最大/最小值

在某些机器上，效率比 `a > b ? a : b` 高。

```cpp
// 如果 a >= b, (a - b) >> 31 为 0，否则为 -1
int max(int a, int b) { return (b & ((a - b) >> 31)) | (a & (~(a - b) >> 31)); }
int min(int a, int b) { return (a & ((a - b) >> 31)) | (b & (~(a - b) >> 31)); }
```

### 判断两非零数符号是否相同

```cpp
bool isSameSign(int x, int y) {  // 有 0 的情况例外
  return (x ^ y) >= 0;
}
```

### 交换两个数

 "该方法具有局限性"
这种方式只能用来交换两个整数，使用范围有限。

对于一般情况下的交换操作，推荐直接调用 `algorithm` 库中的 `std::swap` 函数。

```cpp
void swap(int &a, int &b) { a ^= b ^= a ^= b; }
```

### 操作一个数的二进制位

获取一个数二进制的某一位：

```cpp
// 获取 a 的第 b 位，最低位编号为 0
int getBit(int a, int b) { return (a >> b) & 1; }
```

将一个数二进制的某一位设置为 $0$：

```cpp
// 将 a 的第 b 位设置为 0 ，最低位编号为 0
int unsetBit(int a, int b) { return a & ~(1 << b); }
```

将一个数二进制的某一位设置为 $1$：

```cpp
// 将 a 的第 b 位设置为 1 ，最低位编号为 0
int setBit(int a, int b) { return a | (1 << b); }
```

将一个数二进制的某一位取反：

```cpp
// 将 a 的第 b 位取反 ，最低位编号为 0
int flapBit(int a, int b) { return a ^ (1 << b); }
```

## 内建函数

GCC 中还有一些用于位运算的内建函数：

1. `int __builtin_clz(unsigned int x)` ：返回 $x$ 的二进制的前导 $0$ 的个数。当 $x$ 为 $0$ 时，结果未定义。
2. `int __builtin_ctz(unsigned int x)` ：返回 $x$ 的二进制末尾连续 $0$ 的个数。当 $x$ 为 $0$ 时，结果未定义。
3. `int __builtin_popcount(unsigned int x)` ：返回 $x$ 的二进制中 $1$ 的个数。

这些函数都可以在函数名末尾添加 `l` 或 `ll` （如 `__builtin_popcountll` ）来使参数类型变为 ( `unsigned` ) `long` 或 ( `unsigned` ) `long long` （返回值仍然是 `int` 类型）。

由于这些函数是内建函数，经过了编译器的高度优化，运行速度十分快（有些甚至只需要一条指令）。

【**例题**】`Louise`和`Richard`玩一个游戏。他们有一个初值设置为 N 的计数器。`Louise`先开始游戏，此后两人轮流进行。 在游戏中，他们进行如下操作：

- 如果 N 不是 2 的幂，他们把计数器减少小于 N 的最大2的幂。

- 如果 N 是 2 的次，他们把计数器减少 N 的一半。

得到结果作为新的 N 值，再继续进行以后的操作。
当计数器值减到 1 的时候，即 N == 1，游戏结束。最后一个进行合法操作的人获得胜利。 给定 N ， 你的任务是求出游戏的获胜者。如果`Louise`赢得游戏输出 `"Louise”`。否则输出`”Richard”`。

【**基本思路**】第一种操作等价于去掉高位的第一个1，第二种等价于右移一位。所以只需要判断有几个1和有几个后缀0就可以了。

```c
string counterGame(long long n) {
    int count = __builtin_popcountll(n) + __builtin_ctzll(n);
    return count & 1 ? "Richard" : "Louise";
}
```

## 基础算法

## 二分查找

常用的二分查找场景主要有三种：在一个单调序列中寻找一个数、寻找左侧边界、寻找右侧边界。

二分查找成立的前提是查找目标满足单调性，即单调递减或者递增。取基本思想就是采用分治方法，将查找范围二分为左右两个区间或者加上中间节点，要么中间节点恰好等于目标，程序结束；要么在左区间，要么在右区间，则继续重复二分......所以二分的复杂度是 $O(n)$。

所以二分查找的基本框架如下：

```c
int binarySearch(int nums[], int n, int target) {
  
    int left = 0, right = ...;

    while(...) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            ...
        } else if (nums[mid] < target) {
            left = ...
        } else if (nums[mid] > target) {
            right = ...
        }
    }

    return ...;
}
```

另外注意一下，计算 `mid` 时需要防止溢出，代码中 `left + (right - left) / 2` 就和 `(left + right) / 2` 的结果相同，但是有效防止了 `left` 和 `right` 太大直接相加导致溢出。有些也会将除以2写成移位的形式 `left + ((right - left) >> 1)`。但是要注意的是，移位运算符比算术运算符的优先级低，所以不要忘记加上括号，否则结果不对。

【**注意**】这里计算的 `mid` 是向下取整。

上面代码框架中 ... 标记的部分，就是可能出现细节问题的地方，当你看到一个二分查找的代码时，需要注意这几个地方。下面用实例分析这些地方能有什么样的变化。但是不管怎么变化，只需要在查找时记住一点：**不遗漏，不重复**。

### 寻找一个数（基本的二分搜索）

这种情况最简单，仅需要在一个单调序列中查找**一个数**，如果存在，返回其索引，否则返回 -1。

```c
int binarySearch(int nums[], int n, int target) {
    int left = 0;
    int right = n - 1; // 注意

    while(left <= right) {
        int mid = left + (right - left) / 2;
        if(nums[mid] == target)
            return mid;
        else if (nums[mid] < target)
            left = mid + 1; // 注意
        else if (nums[mid] > target)
            right = mid - 1; // 注意
    }
    return -1;
}
```

1、  `while` 循环的条件中是 `<=` 还是 `<` ？

因为初始化 `right` 的赋值是 `n - 1`，即最后一个元素的索引，而不是 `n`。

这二者可能出现在不同功能的二分查找中，区别是：前者相当于两端都闭区间 `[left, right]`，后者相当于左闭右开区间 `[left, right)`，因为索引大小为 `n` 是越界的。

再这个算法中使用的是前者 `[left, right]` 两端都闭的区间。这个区间其实就是每次进行搜索的区间。

什么时候应该停止搜索呢？当然，找到了目标值的时候可以终止：

```c
if(nums[mid] == target)
    return mid;
```

但如果没找到，就需要 while 循环终止，然后返回 -1。那 while 循环什么时候应该终止？搜索区间为空的时候应该终止，意味着你没得找了，就等于没找到嘛。

`while(left <= right)`的终止条件是 `left == right + 1`，写成区间的形式就是 `[right + 1, right]`，或者带个具体的数字进去 [3, 2]，可见这时候区间为空。所以这时候 while 循环终止是正确的，直接返回 -1 即可。

`while(left < right)` 的终止条件是 `left == right`，写成区间的形式就是 `[left, right]`，或者带个具体的数字进去 [2, 2]，这时候区间非空，还有一个数 2，但此时 while 循环终止了。也就是说这区间 [2, 2] 被漏掉了，索引 2 没有被搜索，如果这时候直接返回 -1 就是错误的。

当然，如果非要用 `while(left < right)` 也可以，只需要在 `while` 循环终止之后，再判断一下最后的区间是否满足条件即可。

```c
//...
while(left < right) {
// ...
}
return nums[left] == target ? left : -1;
```

2、更新搜索区间为什么采用 `left = mid + 1，right = mid - 1`？而有些代码是 `right = mid` 或者 `left = mid`，究竟采用哪一种？

刚才明确了「搜索区间」这个概念，而且本算法的搜索区间是两端都闭的，即 `[left, right]`。那么当我们发现索引 `mid` 不是要找的 `target` 时，下一步应该去搜索哪里呢？

当然是去搜索 `[left, mid-1]`或者 `[mid+1, right]` ，因为 `mid` 已经搜索过，应该从搜索区间中去除。

3、此算法有什么缺陷？

比如有序数组 `nums = [1,2,2,2,3]`，`target` 为 2，此算法返回的索引是 2。但是如果想得到 `target` 的左侧边界，即索引 1，或者想得到 `target` 的右侧边界，即索引 3，这样的话此算法是无法处理的。

这样的需求很常见，可能一个简单的解决方法是：找到一个 `target`，然后向左或向右线性搜索，继续检查是否有相同的 `target`。但是当存在大量的 `target` 时，这个方法就退化为线性查找了，而很难保证二分查找对数级的复杂度了。

下面讨论这两种边界的二分查找算法。

### 寻找左侧边界的二分搜索

避免退化为线性查找的策略是，当找到 `nums[mid] == target` 时，继续在区间 `[left, mid-1]` 查找。

完整代码如下：

```c
int lower_bound(int nums[], int n, int target) {
    int left = 0, right = n - 1;
    // 搜索区间为 [left, right]
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            // 搜索区间变为 [mid+1, right]
            left = mid + 1;
        } 
        else if (nums[mid] > target) {
            // 搜索区间变为 [left, mid-1]
            right = mid - 1;
        } 
        else if (nums[mid] == target) {
            // 收缩右侧边界
            right = mid - 1;
        }
    }
    // 检查出界情况
    if (left >= n || nums[left] != target)
        return -1;
    return left;
}
```

`while` 循环终止的条件是 `left <= right` ，也就是说，当 `while` 循环终止时，`left = right + 1` 。这时有三种情况：

1. 存在 `target` ，这时候 `0 <= left <= n-1`，直接返回 `left` 即可。
2. 不存在 `target` ，并且 `target` 比 `nums` 中所有元素都大，`right = n-1` 始终保持不变，那么终止时，必定会有 `left = n` 。
3. 不存在 `target` ，并且 `target` 比 `nums` 中所有元素都小，那么终止时，必定会有 `left = 0， right = -1` ，这时候只需要判断 `nums[left] == target` 即可。

所以代码中要加上边界判断。

```c
if(left >= n || nums[left] != target)
    return -1;
```

这里运用了 `||` 运算的短路机制，当 `left >= n` 时，不会执行 `nums[left] != target` ，避免了数组越界。注意不能调换顺序。

【**问题**】当我们已经找到目标之后，继续在区间 `[left, mid-1]` 查找，会不会错过了目标？其实不会。分两种情况讨论：

1. 如果区间 `[left, mid-1]` 还有目标，那么一定可以再找到一个目标，不会错过。
2. 如果区间 `[left, mid-1]` 没有目标，这时候我们知道目标值在 `mid` 的位置，`right = mid - 1`，并且区间 `[left, mid-1]` 中所有的值都比 `target` 小，所有 `right` 的值不会变化，最后 `while` 循环结束后，`left = right + 1 = mid`，恰好就是目标值的位置。

### 寻找右侧边界的二分查找

类似寻找左侧边界的算法，当找到 `nums[mid] == target` 时，继续在区间 `[mid+1, right]` 查找。

完整代码如下：

```c
int upper_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } 
        else if (nums[mid] > target) {
            right = mid - 1;
        } 
        else if (nums[mid] == target) {
            // 收缩左侧边界
            left = mid + 1;
        }
    }
    // 检查 right 越界的情况
    if (right < 0 || nums[right] != target)
        return -1;
    return right;
}
```

当 `target` 比所有元素都小时，`right` 会被减到 -1，当 `target` 比所有元素都大时，`right = n - 1` 。

### 注意事项

1. 二分查找一定要求序列满足单调性。
2. 注意「搜索区间」，区分 “左闭右闭”和“左闭右开”区间。
3. 分析 `while` 的终止条件，如果存在漏掉的元素，记得检查边界条件。

### 实数的二分法

实数的二分不像整数二分那样有取整的问题，所以比较简单。

```c
const double eps = 1e-5; // 设定精度

while(right - left > eps) {
    double mid = left + (right - left) / 2;
    if(check(mid)) //check函数根据具体问题检查是否满足条件
        right = mid;
    else
        left = mid;
}
```

有时也可以将 `while` 循环写成 `for(int i  = 0; i < 100; i++)`，采用 `for` 循环执行100次，相对于精度达到 $1/2^{100} \simeq 1/10^{30}$，精度够了。一般循环 $30 \sim 50$ 次是足够的，取决于具体问题。

### 应用

【**题目描述**】给定一个序列，例如 $s = [2,2,3,4,5,1]$，将其划分为三个连续的子序列 $s_1, s_2, s_3$，每一个序列至少包含一个元素，要求每一个子序列的和的最大值最小。

例如，

- 划分1： $s_1=[2,2,3], s_2=[4,5], s_3 = [1]$，子序列的和分别是 $7,9,1$，最大值是 $9$。
- 划分2：$s_1=[2,2,3], s_2=[4], s_3 = [5,1]$，子序列的和分别是 $7,4,6$，最大值是 $7$。

划分2的最大值更小，所以划分2比划分1更好。

【**思路**】每一个子序列至少一个元素，所以子序列和的最大值的最小值要大于序列的最大值，除非划分为一个子序列，否则最大值应该会小于序列之和。所以子序列最大值的范围是 $[max, sum]$，给定这个范围的一个值 $x$，可以去检查是否满足条件。可以用贪心的思想来处理：从左到右尽可能多划分元素给子序列，但是其和不能超过 $x$，如果能够划分为3个子序列，则满足条件；反之，如果超过了3个子序列，则说明这个这个数太小了。如果一个一个枚举，会比较慢，所以可以采用二分法。

```c
#include <bits/stdc++.h>

using namespace std;

bool check(long long a[], int n, long long x)
{
    long long s = 0;
    int count = 1;
    for(int i = 0; i < n; i++) {
        //当前子序列和已经达到最大了，不可以再增加  
        if(s + a[i] > x) {
            count++;
            s = a[i];       // 新的序列
            if(count > 3)   // 划分的子序列超过3个了。
                return false;
        }
        else
            s += a[i];
    }
    return true;
}

int main()
{
    long long a[] = {2,2,3,4,5,1};
    int n = 6;
  
    long long left = 0, right = 0;
    long long ans;
  
    for(int i = 0; i < n; i++) {
        left = max(left, a[i]); // 最大元素
        right += a[i];          // 所有元素之和
    }
  
    while(left <= right) {
        int mid = left + (right - left >> 1);
        if(check(a, n, mid)) {
            right = mid - 1; // 当前值满足条件，看看有没有更小的值
            ans = mid;
        }
        else
            left = mid + 1;
    }
  
    cout << ans;
  
    return 0;
}
```

## 高精度(大数计算)

大数，其实就是很大很大数字(**可能远超32、64位，基础类型无法表示**)的加减乘除和求模等运算，Python和Java自带有大数的类型，很容易解决大数的各种运算，但在C/C++中一般需要手动实现。

一个基本的数据范围估算，32位 `int` 可以处理的范围 $\le 10^9$，64位 `long long` 可以处理的范围 $\le 10^{19}$，如果做乘法，通常需要提高一个量级。有时候可以采用 gcc编译器的 `__int128` 类型，可以处理 $\le 10^{38}$。

这里简单介绍一下 `__int128` 类型。`__int128` 类型不能直接采用 `scanf` 和 `printf` 读写数据，需要自己重写读写方法。下面是一个参考示例。

```c
#include <bits/stdc++.h>
using namespace std;

inline __int128 read()
{
    __int128 x = 0, f = 1;
    char ch = getchar();
    while(ch < '0' || ch > '9') {
        if(ch == '-')
            f = -1;
        ch=getchar();
    }
    while(ch >= '0' && ch <= '9') {
        x = x * 10 + ch - '0';
        ch = getchar();
    }
    return x * f;
 }
 
inline void write(__int128 x)
{
    if(x < 0) {
        putchar('-');
        x = -x;
    }
    if(x > 9)
        write(x / 10);

    putchar(x % 10 + '0');
}
 
int main()
{
     __int128 a = read();
     __int128 b = read();
     write(a + b);
     return 0;
}
```

对于超过 $10^{38}$ 范围的大整数，一般用字符串、数组或链表等形式表示。大数运算的核心就是： **模拟** ，模拟我们日常用纸笔算数字的加减乘除流程，然后再根据计算机、编程语言等特性适当存储计算即可，不过，大数除法运算稍微特殊一点，和我们直接模拟的思维方式稍有不同。

在平常的实现中，高精度数字利用字符串表示，每一个字符表示数字的一个十进制位。因此可以说，高精度数值计算实际上是一种特别的字符串处理。字符串表示的整数，高位在左边，低位在右边，但是为了模拟大整数运算需要对齐，所以可以将字符串反转，也就是说低位存放在数组的左边。例如整数 `1234567`，存放在数组中是 `a[] = {7, 6, 5, 4, 3, 2, 1}`，这样在做运算时，只需要从左到右遍历计算就好了，不需要考虑对齐的问题。

所以读写大整数的代码如下：

```c
#define N 1005

void clear(int a[]) {
    // 注意这里不能采用 memset(a, 0, sizeof(a));
    for (int i = 0; i < N; i++) 
        a[i] = 0;
}

void read(int a[]) {
  static char s[N + 1];
  scanf("%s", s); // 读字符串
  clear(a);
  int len = strlen(s) - 1;
  //反转存放
  for (int i = 0; i <= len; ++i) 
    a[i] = s[len - i] - '0';
}
```

输出也按照存储的逆序输出。由于不希望输出前导零，故这里从最高位开始向下寻找第一个非零位，从此处开始输出；终止条件 i >= 1 而不是 i >= 0 是因为当整个数字等于 0 时仍希望输出一个字符 0。

```c
void write(int a[]) {
    int i = N - 1;
    
    while(i >= 1 && a[i] == 0)
        i--;

    while(i >= 0) 
        putchar(a[i--] + '0');

    putchar('\n');
}
```

大数加法比较简单，只需要注意处理进位即可。这里可以不需要判断位数，直接将数组遍历一遍。

```c
void add(int a[], int b[], int c[]) {
    clear(c);
    for (int i = 0; i < N-1; i++) {
        c[i] += a[i] + b[i];
        //处理进位
        if (c[i] >= 10) {
            c[i + 1] += 1;
            c[i] -= 10;
        }
    }
}
```

减法一样的处理方式，从个位起逐位相减，遇到负的情况则向上一位借 1。但是要注意，减法，只能大数减去小数，所以需要先判断 `a, b` 的大小，特殊处理负号。

```c
int compare(int a[], int b[]) 
{
    int i = N - 1, j = N - 1;
    
    while(i >= 1 && a[i]== 0)
        i--;
    while(j >= 1 && b[j]== 0)
        j--;
    if(i > j)
        return 1;
    if(i < j)
        return -1;
    while(i >= 0) {
        if(a[i] > b[j])
            return 1;
        if(a[i] < b[j])
            return -1;
        i--, j--;
    }

    return 0;
}

void sub(int a[], int b[], int c[]) {
  clear(c);

  for (int i = 0; i < N - 1; ++i) {
    // 逐位相减
    c[i] += a[i] - b[i];
    if (c[i] < 0) {
      // 借位
      c[i + 1] -= 1;
      c[i] += 10;
    }
  }
}
```

对于乘法，高精度乘以单精度比较简单，不需要竖式乘法。

```c
void mul_short(int a[], int b, int c[]) {
  clear(c);

  for (int i = 0; i < N - 1; i++) {
    // 直接把 a 的第 i 位数码乘以乘数，加入结果
    c[i] += a[i] * b;

    if (c[i] >= 10) {
      // 处理进位
      // c[i] / 10 即除法的商数成为进位的增量值
      c[i + 1] += c[i] / 10;
      // 而 c[i] % 10 即除法的余数成为在当前位留下的值
      c[i] %= 10;
    }
  }
}
```

如果两个都是高精度的乘法，则需要采用竖式乘法了。

```c
void mul(int a[], int b[], int c[]) {
    clear(c);
    
   // 逐位相乘求出对应位置的数，先不考虑进位
    for(int i = 0; i < N-1; i++)
        for(int j = 0; j < N-1; j++)
            c[i+j] = c[i+j] + a[i] * b[j];
    // 处理进位
    for(int i = 0; i < N-1; i++) {
        c[i+1] += c[i] / 10; // 求进位，求出第i位向第i+1位的进位
        c[i] %= 10;
    }
}
```

【**注意**】高精度乘法的复杂度是 $O(n^2)$，所以对于更长位数的乘法，可以采用FFT优化。

大数除法需要转换为减法,下列代码可以同时得到商和余数。[参考https://oi-wiki.org/math/bignum/]

```c
// 被除数 a 以下标 last_dg 为最低位，是否可以再减去除数 b 而保持非负
// len 是除数 b 的长度，避免反复计算
bool greater_eq(int a[], int b[], int last_dg, int len) {
    // 有可能被除数剩余的部分比除数长，这个情况下最多多出 1 位，故如此判断即可
    if (a[last_dg + len] != 0) return true;
    // 从高位到低位，逐位比较
    for (int i = len - 1; i >= 0; --i) {
        if (a[last_dg + i] > b[i]) return true;
        if (a[last_dg + i] < b[i]) return false;
    }
    // 相等的情形下也是可行的
    return true;
}

void div(int a[], int b[], int c[], int d[]) {
    clear(c);
    clear(d);

    int la, lb;
    for(la = N - 1; la > 0; --la)
        if(a[la - 1] != 0) break;
    for(lb = N - 1; lb > 0; --lb)
        if(b[lb - 1] != 0) break;
    if(lb == 0) {
        puts("> <");
        return;
    }  // 除数不能为零

    // c 是商
    // d 是被除数的剩余部分，算法结束后自然成为余数
    for (int i = 0; i < la; ++i) d[i] = a[i];
        for (int i = la - lb; i >= 0; --i) {
            // 计算商的第 i 位
            while (greater_eq(d, b, i, lb)) {
                // 若可以减，则减
                // 这一段是一个高精度减法
                for (int j = 0; j < lb; ++j) {
                    d[i + j] -= b[j];
                    if (d[i + j] < 0) {
                        d[i + j + 1] -= 1;
                        d[i + j] += 10;
                    }
                }
                // 使商的这一位增加 1
                c[i] += 1;
                // 返回循环开头，重新检查
            }
        }
    }
```

对于高精度对单精度求模问题，可以简化。

```c
//初始化 mod = 0
//从最右边计算第一位数字的模:
int mod_short(int a[], int mod)
{
    long long sum = a[0];
    int res = 1;
    for(int i = 1; i < N-1; i++){
        res = (res * 10) % mod;
        sum = (sum + res * a[i]) % mod;
    }
    return sum % mod;
}
```

`下面的大数加减法由 ChatGPT 自动生成，仅供参考。`

### 大数加法

大数加法是最简单的，简单模拟即可。首先，我们想一下两个数加法的流程：从右向左计算求和、进位，一直到最后。

在编程语言中同样也是模拟从右向左逐位相加的过程，不过在具体实现上需要注意一些细节。

1、枚举字符串将其转换成 `char[]` 提高效率。

2、从右往左进行计算，可以将结果放到一个数组中最后组成字符串。

3、进位每次叠加过需要清零，两数相加如果大于等于 `10` 即有进位，添加到结果中该位置的数也应该是该数 `%10` 的结果。

4、计算完最后还要看看进位是否为 `1`，如果为 `1` 需要将其添加到结果，例如 `"991"+"11"` 算三个位置为 `002` 但还有一个进位需要添加，所以应该是 `1002`。

```c
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

string add(string num1, string num2) {
    string res = ""; // 存储结果的字符串
    int carry = 0; // 进位标志

    // 使 num1 成为较长的数
    if (num1.length() < num2.length()) {
        swap(num1, num2);
    }

    int i = num1.length() - 1;
    int j = num2.length() - 1;

    while (i >= 0 || j >= 0) {
        int sum = carry;

        if (i >= 0) {
            sum += num1[i] - '0';
            i--;
        }

        if (j >= 0) {
            sum += num2[j] - '0';
            j--;
        }

        carry = sum / 10;
        sum = sum % 10;
        res.push_back(sum + '0');
    }

    if (carry) {
        res.push_back(carry + '0');
    }

    reverse(res.begin(), res.end()); // 反转字符串

    return res;
}

int main() {
    string num1 = "123456789";
    string num2 = "987654321";

    cout << add(num1, num2) << endl; // 输出 1111111110

    return 0;
}
```

### 大数减法

加法对应的就是减法，有了上面大数加法的实现思路，那么我想你在大数减法也应该有点想法，但是减法和加法不同的是减法有位置的区别， **加法需要进位而减法需要借位** 。并且大整正数减法可能产生正负也不一定。

两个正数，如果大数减去小数，那么一切正常，结果是一个正数； **但如果小数减去大数，那么结果将是一个负数，并且结果处理起来比较麻烦** 。 所以在这里全部转成大-小处理(大-小不存在不能借位的情况)。

1、执行计算前首先比较减数 (`num1`) 和被减数 (`num2`) 的大小，如果 `num1>num2`，那么就模拟 `num1-num2` 的过程，如果 `num1<num2`，那么结果就为 `-(num2-num1)` 。当然可以为了稳定模拟时候一个大一个小，可将 `num1` 始终指向较大的那个数，少写一个 `if/else`.

2、在比较两个数字大小的时候，因为是字符形式，首先比较两个字符串的长度，长的那个更大短的那个更小，如果两个字符串等大，那么就可以通过字典序从前往后进行比较。

3、和加法不同的是，减法前面可能产生若干前缀 `0`，这些 `0` 是需要去掉，例如 `"1100"-"1000"` 计算得到的结果位 `"0100"`,你就要把前面的0去掉，返回 `"100"`。

4、具体实现的时候和加法相似，如果使用字符串存储，需要逆置顺序，如果是个负数，前面还要加上 `'-'`.

5、每个位置正常进行减法运算，如果值小于0，那么就需要向上借位(+10)，那么处理上一位进行减法时候还要将借位的处理一下。

```c
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

string subtract(string num1, string num2) {
    string res = ""; // 存储结果的字符串
    int borrow = 0; // 借位标志

    // 使 num1 成为较大的数
    if (num1.length() < num2.length() || (num1.length() == num2.length() && num1 < num2)) {
        swap(num1, num2);
        res.push_back('-'); // 记录结果为负数
    }

    int i = num1.length() - 1;
    int j = num2.length() - 1;

    while (i >= 0 || j >= 0) {
        int diff = borrow;

        if (i >= 0) {
            diff += num1[i] - '0';
            i--;
        }

        if (j >= 0) {
            diff -= num2[j] - '0';
            j--;
        }

        if (diff < 0) {
            borrow = -1;
            diff += 10;
        } else {
            borrow = 0;
        }

        res.push_back(diff + '0');
    }

    // 去除前导零
    while (res.length() > 1 && res.back() == '0') {
        res.pop_back();
    }

    reverse(res.begin(), res.end()); // 反转字符串

    return res;
}

int main() {
    string num1 = "987654321";
    string num2 = "123456789";

    cout << subtract(num1, num2) << endl; // 输出 864197532

    num1 = "123456789";
    num2 = "987654321";

    cout << subtract(num1, num2) << endl; // 输出 -864197532

    return 0;
}
```

### 大数乘法

大数乘法乍一想可能比较复杂，因为乘法比起加法可能进位不光是1，还有两个数各种位置都需要相乘计算，这时候就需要我们**化繁为简**了。

`多*多`考虑起来可能有些麻烦，但是如果 `多*一`考虑起来呢？如果是多位乘以一位数，那么就拿一位的分别乘以多位数的个位、十位、百位，在计算的同时考虑一下进位的情况。

但是也可以先直接用int类型数组存储各位的乘积然后从右向左进行进位，如下图所示。

```c
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

string multiply(string num1, string num2) {
    if (num1 == "0" || num2 == "0") {
        return "0"; // 0 乘以任何数都为 0
    }

    int len1 = num1.length();
    int len2 = num2.length();
    vector<int> tmp(len1 + len2, 0);

    for (int i = len1 - 1; i >= 0; i--) {
        for (int j = len2 - 1; j >= 0; j--) {
            int mul = (num1[i] - '0') * (num2[j] - '0');
            int sum = mul + tmp[i + j + 1];
            tmp[i + j + 1] = sum % 10;
            tmp[i + j] += sum / 10;
        }
    }

    string res = "";
    for (int i = 0; i < len1 + len2; i++) {
        if (i == 0 && tmp[i] == 0) { // 去除前导零
            continue;
        }
        res += to_string(tmp[i]);
    }

    return res;
}

int main() {
    string num1 = "123456789";
    string num2 = "987654321";

    cout << multiply(num1, num2) << endl; // 输出 121932631137021795

    return 0;
}
```

### 大数除法

大数加减乘都搞定了，通过模拟来实现，但是大数除法也通过模拟来实现？

并不是，对于大数 $a/b$，一般最多要求求到其整数解或者余数，即 $a/b=c……d$（$a,b,c,d$ 均为整）;也就是 $a$ 里面有 $c$ 个 $b$ ，并且还剩下 $d$。核心是先求 $c$ 是多少，对于程序来说，可以通过枚举啊，将除法变成减法，从 $a$ 中不断减 $d$，一直到不能减为止。

但是有个问题，如果被除数 $a$ 很大很大，可能有多个 $b$，那么这样时间复杂度太高了，不可能执行那么多次，那么需要怎么样去优化这个方法呢？

那就要加速寻找次数，减少这个减法的次数了，减法次数减小的一个最好方案就是能不能 **扩大除数b** 。如果 $b$ 后面加个 `'0'`，那么算出来的结果就乘以$10$，减法的次数变成原来十分之一。根据这个思想我们可以一直每次找到 $b$ 的最大 $10$ 的倍数(小于 $a$ )计算减的次数再换算成减 $b$ 的总次数，将结果要以字符串方式保留，后面一直迭代到最后为止,这虽然是一道除法运算的题，但是也蕴涵减法和加法(次数叠加到结果中)。

```c
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

// 判断 num1 是否大于等于 num2
bool greaterOrEqual(string num1, string num2) {
    if (num1.length() > num2.length()) {
        return true;
    } else if (num1.length() < num2.length()) {
        return false;
    } else {
        return num1 >= num2;
    }
}

string divide(string dividend, string divisor) {
    if (divisor == "0") {
        return "NaN"; // 除数为 0
    }

    if (dividend == "0") {
        return "0"; // 被除数为 0
    }

    if (!greaterOrEqual(dividend, divisor)) {
        return "0"; // 被除数小于除数
    }

    vector<int> quotient(dividend.length(), 0); // 商
    int len = dividend.length() - divisor.length(); // 商的起始位

    // 使除数和被除数对齐
    divisor += string(len, '0');

    while (greaterOrEqual(dividend, divisor)) {
        int i = len;

        while (greaterOrEqual(dividend, divisor)) {
            dividend = subtract(dividend, divisor); // 用被除数减去除数
            quotient[i]++; // 计算商
        }

        // 如果被除数不足以减去除数，右移除数一位
        divisor.pop_back();
        len--;
    }

    // 构造结果字符串
    string res = "";
    bool leadingZero = true; // 是否为前导零

    for (int i = 0; i < quotient.size(); i++) {
        if (quotient[i] == 0 && leadingZero) { // 去除前导零
            continue;
        } else {
            leadingZero = false;
        }

        res += to_string(quotient[i]);
    }

    if (res == "") { // 如果商为空，则商为 0
        res = "0";
    }

    return res;
}

int main() {
    string dividend = "123456789";
    string divisor = "987654321";

    cout << divide(dividend, divisor) << endl; // 输出 0

    dividend = "987654321";
    divisor = "123456789";

    cout << divide(dividend, divisor) << endl; // 输出 8

    return 0;
}
```

### 大数求模

大数求模是指计算 $m \% n$，一般 $m$ 是用字符串表示的大整数，而 $n \le 10^{18}$。 可以按照下来伪代码来实现：

```c
//初始化 mod = 0
//从最右边计算第一位数字的模:
mod = (mod * 10 + digit) % m
quo[i] = mod / m
```

如果两个都是大整数，则代码如下：

```c
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

// 判断 num1 是否大于等于 num2
bool greaterOrEqual(string num1, string num2) {
    if (num1.length() > num2.length()) {
        return true;
    } else if (num1.length() < num2.length()) {
        return false;
    } else {
        return num1 >= num2;
    }
}

string mod(string dividend, string divisor) {
    if (divisor == "0") {
        return "NaN"; // 除数为 0
    }

    if (dividend == "0") {
        return "0"; // 被除数为 0
    }

    if (!greaterOrEqual(dividend, divisor)) {
        return dividend; // 被除数小于除数，直接返回被除数
    }

    int len = dividend.length() - divisor.length(); // 商的起始位

    // 使除数和被除数对齐
    divisor += string(len, '0');

    while (greaterOrEqual(dividend, divisor)) {
        while (greaterOrEqual(dividend, divisor)) {
            dividend = subtract(dividend, divisor); // 用被除数减去除数
        }
        // 如果被除数不足以减去除数，右移除数一位
        divisor.pop_back();
        len--;
    }

    return dividend;
}

int main() {
    string dividend = "123456789";
    string divisor = "987654321";

    cout << mod(dividend, divisor) << endl; // 输出 123456789

    dividend = "987654321";
    divisor = "123456789";

    cout << mod(dividend, divisor) << endl; // 输出 9

    return 0;
}
```

## 区间操作

给定一个长度为 $n$ 的数组 $a$， 区间操作一般有四种：1）单点查询、2）区间查询、3）单点修改和 4）区间修改。

```c
query(int a[], int n, int i);
query_range(int a[], int n, int l, int r);
update(int a[], int n, int i, int v);
update_range(int a[], int n, int l, int r, int v);
```

采用普通数组来实现这四个操作的复杂度，假定这里的查询仅仅是指求和操作。

1）单点查询，$O(1)$，直接返回 $a_i$。

2）区间查询，$O(n)$，遍历 $\sum_{i=l}^r a_i$。

3）单点修改，$O(1)$，直接修改 $a_i = v$。

4）区间修改，$O(n)$，遍历 $a_i = a_i + v, \forall i \in [l, r]$。

### 前缀数组

前缀和可以简单理解为「数列的前 $n$ 项的和」，是一种重要的预处理方式，能大大降低查询的时间复杂度。

【**问题**】输入一个长度为 $n$ 的整数序列。接下来再输入 $q$ 个询问，每个询问输入一对 $l, r$。对于每个询问，输出原序列中从第 $l$ 个数到第 $r$ 个数的和。

【**暴力解法**】对每一次询问，遍历区间求和。

```c
int n, q;

scanf("%d%d",&n, &q);
for(int i = 1; i <= n; i++)
    scanf("%d", a+i);

while(q--)
{
    int l, r;
    int sum = 0;
    scanf("%d%d", &l, &r);
    for(int i = l; i <= r; i++)
    { 
        sum += a[i];
    }
    printf("%d\n",sum);
}
```

显然，暴力解法的时间复杂度是 $O(q*n)$。

【**优化思路**】可以采用前缀数组做预处理。

定义一个 $s$ 数组，$s_i$ 代表 $a$ 数组中前 $i$ 个数的和，$s_i= s_{i-1} + a_i$。所以区间 $[l, r]$ 的和为 $s_r - s_{l-1}$。可以看到，通过预处理之后，计算区间和的时间复杂度是 $O(1)$，预处理是 $O(n)$。

注意边界，一般为了方便处理，数组下标从1开始。

```c
int s[N];
int n, q;

scanf("%d%d", &n, &q);
for(int i = 1; i <= n; i++) { 
    scanf("%d", a);
    s[i] = s[i-1] + a;
}

while(q--) {
    int l, r;
    scanf("%d%d", &l, &r);
    printf("%d\n", s[r] - s[l-1]);
}
```

### 二维前缀和

同一维前缀和一样，定义 $s_{i,j}$ 表示二维数组中，左上角 $(1,1)$ 到右下角 $(i,j)$ 所包围的矩阵元素的和。根据容斥原理，容易得到 $s_{i,j} = s_{i-1, j} + s_{i, j-1} - s_{i-1, j-1} + a_{i,j}$。经过预处理后，子区域 $(i_1, j_1), (i_2, j_2)$的和为
$s_{i_2,j_2} - s_{i_1-1,j_2}-s_{i_2, j_1-1} + s_{i_1-1, j_1 - 1}$。

```c
for(int i = 1; i <= n; i++)
    for(int j = 1; j <= m; j++)
        s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + a[i][j];
```

二位前缀和预处理时间复杂度是 $O(nm)$，查询是 $O(1)$。

二维前缀和思想，在计算机图形学领域，最早由 Frank Crow 在1984年首次提出，是为了在多尺度透视投影中提高渲染速度。积分图算法是一种快速计算图像区域和以及图像区域平方和的算法，后面在计算机视觉中，比如人脸检测等诸多领域得到广泛应用。

### 差分数组

【**问题背景**】给定区间 $[l ,r ]$，把 $a$ 数组中的 $[l, r]$ 区间中的每一个数都加上 $v$，即 $a[i] + v, \forall i \in [l,r]$。经过多次操作，要查询变化之后 $a$ 数组的元素。

暴力做法是循环每一个区间，时间复杂度 $O(n)$，如果需要对原数组执行 $q$ 次这样的操作，时间复杂度就会变成 $O(qn)$。这种情况可以考虑差分数组来优化。

【**差分**】差分可以看作是求和的逆运算，就像微分和积分的对应关系。
定义 $d_i = a_i - a_{i-1}$。显然有

$$
\begin{aligned}
d_1 &= a_1 - a_0\\
d_2 &= a_2 - a_1\\
&\cdots\\
d_{i-1} &= a_{i-1} - a_{i-2}\\
d_{i} &= a_{i} - a_{i-1}
\end{aligned}

$$

所以 $a_i = \sum_{k=0}^i d_i$。差分数组的前缀和就是原来的数组。

差分数组可以快速进行区间增减的操作，比如，对区间 $a[i...j]$的全部加上 $v$，那么只需要操作 $d_i = d_i + v,d_{j+1}=d_{j+1}-v$，区间内的元素变化一致，所以差分保持不变。

通过差分数组也容易得到 $a$ 数组的前缀和 $s_i = \sum_{k=1}^k a_k = \sum_{k=1}^i \sum_{j=1}^k d_j = \sum_{k=1}^i(i-k+1)d_k$。

通过差分数组，可以 $O(1)$ 的时间复杂度完成区间修改。

```c
for(int i = 1; i <= n; i++)
    d[i] = a[i] - a[i-1]

while(q--) {
    int l, r, v;
    scanf("%d%d%d", &l, &r, &v);
    d[i] += v;
    d[j+1] -= v;
}
```

### 二维差分

很容易将一维差分推广到二维差分。对于原矩阵某个位置的值，等价于以这个元素为右下角，原矩阵左上角为左上角的子矩阵中所有元素的和。

```c
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= m; j++) {
        d[i][j] = a[i][j] - a[i-1][j] - a[i][j-1] + a[i-1][j-1];
    }
}
```

对子区域$[(i,j),(x,y)]$ 的修改，仅影响四个端点的值。

```c
void update(int i, int j, int x, int y, int val) {
    d[i][j] += val, d[x+1][y+1] += val;
    d[x+1][j] -= val, d[i][y+1] -= val;
}
```

和一维差分一样，计算差分矩阵的前缀和就可以得到原矩阵的元素 $a_{ij}$。

```c
for (int i = 1; i <= n; i++)
    for (int j = 1; j <= m; j++)
        a[i][j] = d[i][j] + d[i-1][j] + d[i][j-1] - d[i-1][j-1];
```

## 数论基础

数论（number theory）是研究整数的一个数学分支。

## 整除/不可整除

【定义】一般我们说整数 $a$ 整除 $b$ 蕴含的意思是 $b$ 能够被 $a$ 除尽。数学定义是，$a$ 整除 $b$ （记作 $a \mid b$），当且仅当存在整数 $k$，使得 $ak = b$。

根据整除的定义，容易得到整除的一些性质：

1. $(a \mid b) \land (b \mid c) \implies a \mid c$
2. $(a \mid b) \land (a \mid c) \implies \forall s, t \in \mathbb{Z}, a \mid (sa + tb)$
3. $c \neq 0 \land a \mid b \Longleftrightarrow ca \mid cb$

【除法定理】如果一个数不能恰好整数另一个整数时，会得到一个商(Quotient) 和 一个剩下的余数（remainder），更准确地说：如果 $n, d \in \mathbb{Z}$，且 $d \neq 0$， 那么一定存在唯一的整数对 $q, r$，使得 $n = q\cdot d + r$。整数 $r$ 就是余数。容易知道余数 $0 \le r < d$。

【**注意**】数学上的余数和我们程序语言的余数有点不太一致，程序设计语言的余数可能是负数，但是数学上按照惯例都是非负整数。

## 约数

根据整除的定义，如果 $b \mid a$，则称 $b$ 是 $a$ 的约数。

1. 试除法求一个数的所有约数

   找出一个整数 $n$ 的所有约数，只需要在 1 到 $n/i$ 中去找较小的那个约数即可，较大的那个约数可以通过计算得到。

   ```c
   void get_divisor(int divisor[], int n)
   {
       for(int i = 1, k = 0; i <= n / i; i++){
           if( n % i == 0){
               divisor[k++] = i;
               if(i != n / i)
                   divisor[k++] = n / i;
           }
       }
   }
   ```

2. 约数个数

   因为对于任意一个合数 $n$ 我们都可以将其分解为 $\prod_{i=1}^kp_i^{\alpha_i}$ 的形式，根据乘法原理，一个合数的约数个数应为 $\prod_{i=1}^k{(\alpha_i + 1)}$，因为每个质因数的次数都可以从0取到 $\alpha_i$ ，因此有这么多种组合。
   例如 $n=12$，显然 $12 = 2^2 \times 3^1$，所有共有 $6$ 个约数，分别是 $\{1,2,3,4,6,12\}$。

3. 约数之和

   合数 $n$ 的约数之和为 $\prod_{i=1}^k(p_i^{0} + p_i^{1} + \cdots + p_i^{\alpha_i})$，该式展开后就是各个约数相加。

## 最大公约数

【定义】对于两个整数 $a$ 和 $b$，如果 $d \mid a$ 且 $d \mid b$，则 $d$ 是 $a$ 和 $b$ 的公约数，其中最大的约数称为最大公约数，记作 $\gcd(a, b)$。

最大公约数的性质：

- $\gcd(a, b) = \gcd(a, ka+b)$
- $\gcd(ka, kb) = k\gcd(a, b)$
- $\gcd(a, b, c) = \gcd(\gcd(a, b), c)$

1. 欧几里得算法（辗转相除法）

```c
//递归
int gcd(int a, int b) {
    return b ? gcd(b, a % b) : a;
}

//迭代
int gcd(int a, int b) {
    if(b) while(a %= b && b %= a);
    return a + b;
}
```

2. 更相减损术

更相减损术是出自《九章算术》的一种求最大公约数的算法。

    可半者半之，不可半者，副置分母、子之数，以少减多，更相减损，求其等也。以等数约之。

```c
int gcd(int a, int b){
    while(a != b) {
        while(a > b) a -= b;
        while(b > a) b -= a;
    }
    return a;
}
```
更相减损术相比于欧几里得算法，计算次数多非常多，但好处是可以避免做除法，如果高精度的除法耗时比较多时，可以考虑采用这个方法。

3. Stein算法

Stein算法是对更相减损术的一种改进，前提是 $a \ge b$，分为几种情况：
- $a,b$ 都是偶数，则 $\gcd(a, b) = 2\gcd(a/2, b/2)$（性质2）
- $a$ 是偶数, $b$ 是奇数，则 $\gcd(a, b) = \gcd(a/2, b)$（性质1）
- $a$ 是奇数, $b$ 是偶数，则 $\gcd(a, b) = \gcd(a, b/2)$（性质1）
- $a,b$ 都是奇数，则 $\gcd(a, b) = 2\gcd((a+b)/2, (a-b)/2)$（性质1）

整数的奇偶判断可以用与运算，除2可以用右移实现，所以Stein算法只有加减法和移位运算。

```c
int gcd_stein(int a, int b) {
    if(a == b) 
        return a;
    // a < b 交换
    if(a < b)
        return gcd_stein(b, a);

    // a, b 偶数
    if(!(a & 1) && !(b & 1))
        return 2*gcd_stein(a >> 1, b >> 1);
    // a 偶数
    else if(!(a & 1))
        return gcd_stein(a >> 1, b);
    // b 偶数
    else if(!(b & 1)) 
        return gcd_stein(a, b >> 1);
    // a, b 奇数
    else
        return gcd_stein(a + b >> 1, a - b >> 1);
}
```

【裴蜀定理】(Bezout's Lemma) 对任意整数 $a$ 和 $b$，则存在整数 $x$ 和 $y$，使得 $ax + by = gcd(a,b)$。

【丢番图方程】方程 $ax+by=c$ 有整数解当且仅当 $\gcd(a, b) \mid c$。如果有整数解，则有无穷多个整数解。设 $x_0, y_0$ 是一个特解，那么所有的解是 $x = x_0 + (b/d)k, y = y_0 - (a/d)k$，其中 $d = \gcd(a, b), k \in \mathbb{Z}$。

如何 $ax+by=c$ 找到一个特解呢？可以通过扩展欧几里得算法。

```c
// 返回 d = gcd(a, b)，通过得到 ax + by = d 的一组特解。
int extend_gcd(int a, int b, int& x, int& y) {
    if(b == 0) {
        x = 1; 
        y = 0;
        return a;
    }
    int d = extend_gcd(b, a % b, y, x);
    y -= a / b * x;
    return d;
}
```
通过扩展欧几里得算法得到特解 $x_0, y_0$，那么 $ax+by=c$ 容易得到是 $x_0' = x_0 c /d, y_0' = y_0 c/d$。

【例题】《虎胆龙威3》中有一个经典的倒水问题，需要用一个 3 加仑的桶和一个 5 加仑的桶，准确装出 4 加仑的水。只能有三个操作：

- 倒满一个桶

- 倒空一个桶

- 将一个桶的水倒到另一个桶，直到一个桶为空或者一个桶满了。
  
![](https://pic1.zhimg.com/80/v2-2fd4c34136ef31507dd52a159e508034_720w.webp)

【解题思路】令 $a = 3, b = 5$，用数学归纳法很容易证明任意时刻，桶里的水都是 $a,b$ 的线性组合，即 $ax + by$。所以目标是4加仑的水，就是求解方程 $3x + 5y = 4$。根据上面的定理，$\gcd(3, 5) = 1 \mid 4$，所以显然是有解的。如果 $x$ 是负整数，则表示倒空 $|x|$；如果是正整数，则表示要倒满 $|x|$ 次。$y$ 同理。

一个最少步骤的基本操作原则：1）如果小桶空，装满；2）如果大桶满了，倒空；3）小桶往大桶倒水，直到小桶空或者大桶满。



## 同余

【定义】若整数 $a$ 和 $b$ 除以整数 $m$ 的余数相同，则称 $a$ 和 $b$ 对于模 $m$ 同余，记作 $a \equiv b \pmod m$。

【定理】$a \equiv b \pmod m$ 当且仅当 $m \mid (a - b)$。

例如 $3 \mid (13 - 4)$，所以 $13 \equiv 4 \pmod 3$，余数都是1。

同余是一种等价关系，满足自反性、对称性和传递性。即

- $a \equiv a \pmod m$。
- 若 $a \equiv b \pmod m$，则 $b \equiv a \pmod m$。
- 若 $a \equiv b \pmod m$，且 $b \equiv c \pmod m$，则 $a \equiv c \pmod m$。

若 $a \equiv b \pmod m$，且 $c \equiv d \pmod m$，则同余运算规则有：

- 加法：$a + c \equiv b + d \pmod m$。
- 减法：$a - c \equiv b - d \pmod m$。
- 乘法：$ac \equiv bd \pmod m$。
- 幂方：若 $k \ge 0, m \ge 0$，$a^k \equiv b^k \pmod m$。

**同余运算两边同时除以同一个整数，不一定能保持同余关系**。也就是说 $ak \equiv bk \pmod m$，不一定可以得到 $a \equiv b \pmod m$。例如 $9 \times 2 \equiv 3 \times 2 \pmod 4$，但是 $9 \ne 3 \pmod 4$。而 $9 \times 2 \equiv 3 \times 2 \pmod 3$，但是 $9 \equiv 3 \pmod 3$

【定理】当 $\gcd(k, m) = 1$ 时， $ak \equiv bk \pmod m \Longleftrightarrow a \equiv b \pmod m$。

【**逆**】给定整数 $a$，若 $gcd(a, m)= 1$，则 $ax \equiv 1 \pmod m$ 的一个解是 $a$ 模 $m$ 的逆，记作 $a^{-1}$。

要注意的是，并不是任意整数都存在逆，并且如果有逆，并不是唯一的。例如 $5x \equiv 1 \pmod 7$，$x = 10$ 是一个解，所以 $10$ 是 $5$ 模 $7$ 的 逆，当然，24也是$5$ 模 $7$ 的逆。

现在问题是，如果存在逆，如何求解？第一种方法是用扩展欧几里得算法求逆。因为 $ax \equiv 1 \pmod m$ 可以转换为解方程 $ax + my = 1$。第二种方法是用费马小定理。

```c
long long mod_inverse(long long a, long long m) {
    long long x, y;
    extend_gcd(a, m, x, y);
    return (x % m + m) % m; // 这一步可以保证是最小的正数。
}
```
【**费马小定理**】 对于正整数 $a$ 和质数 $p$，如果 $p$ 不能整除 $a$，那么， $a^{p-1} \equiv 1 \pmod p$。

所以根据费马小定理，$a$ 模 $p$ 的逆就是 $a^{n-2}$。用快速幂就可以解决。

**取模的除法** $a/b \bmod m = ab^{-1} \bmod m = (a \bmod m) (b^{-1} \bmod m) \bmod m$


## 质数

质数又称素数。一个大于1的自然数，除了1和它自身外，不能被其他自然数整除的数叫做质数；否则称为合数（规定1既不是质数也不是合数）。

质数的个数是无穷的。欧几里得的《几何原本》中有一个经典的证明。它使用了证明常用的方法：反证法。具体证明如下：假设质数只有有限的$n$ 个，从小到大依次排列为 $p_1, p_2, \cdots, p_n$ ，设 $N=p_1 \times p_2\times \cdots \times p_n + 1$，那么，$N$ 是素数或者不是素数。

1. 如果$N$ 为素数，则 $N$ 要大于 $p_1, p_2, \cdots, p_n$，所以它不在那些假设的素数集合中。
2. 如果 $N$ 为合数，因为任何一个合数都可以分解为几个素数的积；而 $N$ 和 $N-1$ 的最大公约数是 $1$，所以不可能被 $p_1,p_2, \cdots, p_n$ 整除，所以该合数分解得到的素因数肯定不在假设的素数集合中。

因此无论该数是素数还是合数，都意味着在假设的有限个素数之外还存在着其他素数。所以原先的假设不成立。也就是说，素数有无穷多个。

### 质数判定方法

1. 暴力做法 $O(n)$

   做法：从 $2 \sim n$ 遍历找约数，如果不存在约数则 $n$ 为质数.
2. 优化做法 $O(\sqrt{n})$

   性质：如果 $i$ 能整除 $n$，那么 $n/i$ 也能整除 $n$，即约数是成对出现的。 因此我们只需要去找每一对质因子中较小的那个质因子即可，所以我们遍历的时候只需要从 $2$ 枚举到 $n/i$ 即可，这样时间复杂度可以降低到 $O(\sqrt{n})$。

   ```c
   bool is_prime(int n)
   {
       if(n <= 1)
           return false;

       int upbound = sqrt(n);

       for(int i = 2; i <= upbound; i++)
           if(n % i == 0)
               return false;

       return true;
   }
   ```

### 质数筛法

筛法可以计算出 $2 \sim n$ 之间所有的质数。

1. 朴素筛法 $O(n\log n)$

   从 $2 \sim n$ 枚举，对于每个 $i$ 我们都把它的所有倍数都筛掉，这样剩下未被筛掉的数就是质数。（因为如果没有被筛掉说明在 $2 n$ 中不存在其的约数，即为质数）。

   ```c
   int primes[N];
   bool is_prime[N];

   void naive_sieve(int n){
       int p = 0;

       for(int i = 0; i <= n; i++)
           is_prime[i] = true;

       is_prime[1] = is_prime[0] = false;

       for(int i = 2; i <= n; i++){
           if(is_prime[i])
               primes[p++] = i;

           for(int j = i + i; j <= n; j += i)
               is_prime[j] = false;
       }
   }
   ```

2. 埃氏筛法 $O(n\log \log n)$

   通过观察我们可以发现，我们其实没有必要把每个数的倍数都筛一遍，只需要把所有质数的倍数筛掉即可，因为所有合数都是由质数组合成的（合数分解）。

   ```c
   int primes[N];
   bool is_prime[N];

   void eratosthenes_sieve(int n){
       int p = 0;

       for(int i = 0; i <= n; i++)
           is_prime[i] = true;

       is_prime[1] = is_prime[0] = false;

       for(int i = 2; i <= n; i++){
           if(is_prime[i]) {
               primes[p++] = i;
               for(int j = i + i; j <= n; j += i)
                   is_prime[j] = false;
           }
       }
   }
   ```
3. 线性筛法 $O(n)$

   我们之前在判断质数的方法中提到过，质因子都是成对出现的，因此同样我们只需要枚举较小的那个质因子即可（即所有合数都只会被它的最小质因子筛掉） 所以我们需要对每一个 $i$ 都从之前的质数中去找它的最小质因子，找到了就 `break` 掉。

   ```c
   int primes[N];
   bool is_prime[N];
   void linear_sieve(int n){
       int p = 0;

       for(int i = 0; i <= n; i++)
           is_prime[i] = true;

       is_prime[1] = is_prime[0] = false;

       for(int i = 2; i <= n; i++){
           //i每次更新都要把2~i之间的数筛一遍，且我们只找较小的那个约数
           if(is_prime[i])
               primes[p++] = i;

           for(int j = 0; primes[j] <= n / i; j++){
               //顺便筛掉质数的倍数
               is_prime[primes[j] * i] = false;
               //找到i的最小质因子，加上这个优化就会变为线性的
               if(i % primes[j] == 0)
                   break;
           }
        }
   }
   ```

### 大质数判定

**Wilson定理** 若整数 $n$ 是质数，当且仅当 $(n-1)! \equiv 1 \pmod n$ 。

`Wilson` 定理给出了一个整数 $n$ 是否是质数的充分必要条件。但是对于大整数的判定，计算量太大。

根据 `Fermat` 定理，给定一个整数 $a$，如果 $a^{n-1} \neq 1 \pmod n$，那么 $n$ 肯定是合数。但是满足这个条件，并不一定是质数。如果随机选择多个整数 $a$，经过判定都满足条件，那么 $n$ 很大概率是一个质数。但是存在一些合数，取任何整数 $a$ 都能通过测试。这种数称为 Carmichael 数。最小的 Carmichael 数是561。

Miller–Rabin素性检验（Miller–Rabin primality test）是一种素数判定法则，利用随机化算法判断一个数是合数还是可能是素数。1976年，卡内基梅隆大学的计算机系教授Gary Miller首先提出了基于广义黎曼猜想的确定性算法，由于广义黎曼猜想并没有被证明，于1980年，由以色列耶路撒冷希伯来大学的Michael Oser Rabin教授作出修改，提出了不依赖于该假设的随机化算法。

Miller–Rabin素性检验依赖于 `Fermat` 小定理和二次探测定理。

**二次探测定理** 若 $p$ 是奇质数，那么 $x^2 \equiv 1 \pmod p$ 没有非平凡平方根，即 $x = 1$ 或者 $x = p-1$。

证明：$x^2 \equiv 1 \pmod p$，等价于 $(x+1)(x-1) \equiv 0 \pmod p$，所以  $p \mid (x+1)(x-1)$，根据欧几里得引理，$x-1 \equiv 0 \pmod p$ 或 $x+1 \equiv 0 \pmod p$，所以 $x = 1$ 或者 $x = p-1$。

也就是说，如果对模 $n$ 存在 $1$ 的非平凡根，则 $n$ 是合数。

现在假定 $n$ 是奇质数，那么 $n-1$ 是偶数，可以表示为 $n - 1 = 2^s d$，根据 `Fermat` 小定理，$a^{2^s d} \equiv 1 \pmod n$，那么对于系列 $a^{2^{s-1}d},\cdots ,a^{2d},a^{d}$，根据二次探测定理，如果 $a^{2^id} \equiv n-1 \pmod n, 1 \le i \le s$，那么 $n$ 可能素数，如果 $a^{2^id} \equiv 1 \pmod n, 1 \le i \le s$
，那么继续迭代，最后可以得到 $a^{d} \equiv 1 \pmod n$。

```c
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
inline ll pw(ll a, ll b, ll p) {
    ll res = 1;
    while (b > 0) {
        if (b & 1)
            res = (__int128)res * a % p;
        b >>= 1;
        a = (__int128)a * a % p;
    }
    return res;
}
inline bool miller_rabin(ll n) {
    if (n < 3 || !(n & 1))
        return n == 2;
    ll t = n - 1, k = 0;
    while (!(t & 1))
        t >>= 1, ++k;
    for (int i = 1, j; i <= 7; i++) {
        ll x = rand() % (n - 2) + 2, v = pw(x, t, n);
        if (v == 1)
            continue;
        for (j = 0; j < k; j++) {
            if (v == n - 1)
                break;
            v = (__int128)v * v % n;
        }
        if (j == k)
            return false;
    }
    return true;
}
```

### 分解质因数

对于每个合数 $n$ 我们都可以将其分解成 $\prod_{i=1}^kp_i^{\alpha_i}$ 的形式,其中 $p_i$ 为 $n$ 的质因子，这个过程我们成为分解质因数。

1. 暴力做法 $O(n)$

   从 $2 \sim n$ 中枚举所有数，如果 `n % i == 0`，再用 `while` 去求该质因子的幂次。
2. 优化算法 $O(\sqrt{n})$

   $n$ 中最多只包含一个大于 $\sqrt{n}$ 的质因子。 因此我们枚举的时候只需要枚举 $2 \sim \sqrt{n}$ 即可，最后如果 $n > 1$，则剩下的这个就是 $n$ 中那个大于 $\sqrt{n}$ 的质因子。

   ```c
   void divisor(int n)
   {
       int upbound = sqrt(n);
       for(int i = 2; i <= upbound; i++) {
           int s = 0;
           if(n % i == 0) {
               while(n % i == 0){
                   n /= i;
                   s++;
               }
               printf("%d^%d\n", i, s);
           }
       }

       if(n > 1)
           printf("%d^%d\n", n, 1);
   }
   ```

## 欧拉函数

【**定义**】欧拉函数 $\varphi(n)$  表示在 $1 \sim n$ 中与 $n$ 互质的整数的个数。

### 计算 $\varphi(n)$

- 性质1 若 $p$ 是质数，则 $\varphi(p) = p-1$。
- 性质2 若 $n, m$ 互质，则 $\varphi(nm) = \varphi(n) \varphi(m)$
- 性质3 若 $p$ 是质数，则 $\varphi(p^k) = p^k - p^{k-1}$。
- 性质4 当 $n > 2$，$\varphi(n)$是偶数。

  证明：若 $(x,n) = 1$，那么 $(n−x ，n) = 1$，所以与 $n$ 互质的数成对出现。

根据算术基本原理，任意非负整数可以分解为

$$
n = \prod_{i=1}^kp_i^{\alpha_i}

$$

所以，由性质1~3，容易得到

$$
\varphi(n) = \prod_{i=1}^k p_i^{\alpha_i}(1-\frac{1}{p_i}) = n(1 - \frac{1}{p_1})(1 - \frac{1}{p_2})\cdots(1 - \frac{1}{p_k})

$$

1. 普通方法求欧拉函数 $O(\sqrt{n})$

   根据公式 $\varphi(n) = n(1 - \frac{1}{p_1})(1 - \frac{1}{p_2})\cdots(1 - \frac{1}{p_k})$，分解整数 $n$ 的质因数 $p_i$。

   ```c
   int phi(int n){
       int res = n;
       for(int i = 2; i <= n / i; i++){
           if(n % i == 0){
               //为了防止出现res*(1-1/i)出现小数，因此先除i再乘上(i-1)
               res = res / i * (i - 1);
               while(n % i == 0){
                   n /= i;
               }
           }
       }

       if(n > 1)
           res = res / n * (n-1);

       return res;
   }
   ```

2. 线性筛法求欧拉函数 $O(n)$

   该方法用于求 $1 \sim n$ 所有数的欧拉函数值。线性筛法求欧拉函数的思想与之前利用线性筛法求质数差不多，只不过这里我们需要分情况来讨论。

   (1) 如果 $i$ 为质数那么 $\varphi(i) = i-1$  ，因为 $1 \sim i-1$ 之间的数都与 $i$ 互质。

   (2) 如果 $i$ 不为质数，$p = primes[j] * i$ ，$i \mid primes[j]$时

   某一个数的欧拉函数值跟其分解质因数后质因数的次数无关，因为 $primes[j]$ 是 $i$ 的一个质因子，因此在 $i$ 的欧拉函数中已经出现了 $(1-\frac{1}{prime[i]})$ 这一项了，而 $i$ 和 $p$ 只相差 $primes[j]$ 倍，所以它们分解质因数的结果只相差 $primes[j]$ 的一次方，因此是它们的欧拉函数之间也只相差 $primes[j]$ 倍，即 $\varphi(i) = i*\prod_{i=1}^n p_i^{\alpha_i}* \varphi(p) = primes[j] *i* \prod_{i=1}^n p_i^{\alpha_i}= primes[j]*\varphi(i)$

   (3) 如果 $i$ 不为质数，$p = primes[j] * i$ ，$i \nmid primes[j]$ 时

   这时 $primes[j]$ 不是 $i$ 的一个质因子，$i$ 和 $p$ 的欧拉函数就会相差 $primes[j] * (1-\frac{1}{prime[i]})$ 这么多倍。

   ```c
   int primes[N]
   int phi[N];
   bool is_prime[N];
   void eulers(int n)
   {
       phi[1] = 1; //特殊规定
       int p = 0;

       for(int i = 2;i<=n;i++) {
           if( is_prime[i]) {
               primes[p++] = i;
               phi[i] = i-1;
           }
           for(int j = 0; primes[j] <= n / i; j++) {
               is_prime[primes[j]*i] = false;
               if(i % primes[j] == 0){
               //公式成立必须是primes[j]也是i的质因子才行
                   phi[primes[j] * i] = phi[i] + primes[j];
                   break;
               }
               phi[primes[j]*i] = phi[i]*(primes[j]-1);
           }
       }
   }
   ```

### 欧拉函数的应用

- **欧拉定理** 若正整数 $a$ 与 $n$ 互质，则 $a^{\varphi(n)} \equiv 1 \pmod n$。

  证明：根据欧拉函数的定义，小于 $n$ 且与 $n$ 互质的数有 $\varphi(n)$ 个，现在设这 $\varphi(n)$ 个数分别是 $u_1, u_2, \cdots, u_{\varphi(n)}$，显然 $u_i < n, \forall i \in 1, 2, \cdots, \varphi(n)$。

  现在考察这 $\varphi(n)$ 个数与 $a$ 相乘后得到的新的 $\varphi(n)$ 个数，为$au_1, au_2, \cdots, au_{\varphi(n)}$，它们中任意两个数的差都不能被 $n$ 整除。这是因为任意两个数的差是 $au_i - a u_j = a(u_i - u_j)$，其中 $a$ 与 $n$ 互质，而 $u_i - u_j < n$。

  这意味着新的 $\varphi(n)$ 个数除以 $n$ 得到的余数两两不相同。

  另外，由于每个 $u_i$ 都与 $n$ 互质，$a$ 也与 $n$ 互质，这就是说每个 $au_i$ 都与 $n$ 互质。我们知道，与 $n$ 互质的数除以 $n$ 得到的余数也必然与 $n$ 互质。也就是说，这新的 $\varphi(n)$ 个数除以 $n$ 得到的余数都是小于 $n$ 且与 $n$ 互质的数。

  而小于 $n$ 且与 $n$ 互质的数正好就是 $u_1, u_2, \cdots, u_{\varphi(n)}$。
  所以我们知道，新的 $\varphi(n)$ 个数除以 $n$ 的余数就是 $u_1, u_2, \cdots, u_{\varphi(n)}$的一个排列。
  所以

  $$
  u_1 \cdot au_2 \cdot \cdots \cdot au_{\varphi(n)} \equiv u_1 \cdot u_2 \cdot \cdots \cdot u_{\varphi(n)}  \pmod n

  $$

  即

  $$
  a^{\varphi(n)} u_1 \cdot u_2 \cdot \cdots \cdot u_{\varphi(n)} \equiv u_1 \cdot u_2 \cdot \cdots \cdot u_{\varphi(n)}  \pmod n

  $$

  因为，$u_i$ 与 $n$ 互质，所以

  $$
  a^{\varphi(n)} \equiv 1 \pmod n

  $$
- **费马小定理** 对于正整数 $a$ 和质数 $p$，如果 $p$ 不能整除 $a$，那么， $a^{p-1} \equiv 1 \pmod p$。
- **定理** $n$ 的所有因子的欧拉函数之和等于 $n$，即 $\sum_{d \mid n}\varphi(d) = n$。

  例如，$n=10$，那么 $n$ 的因子有 $1、2、5、10$。容易计算得到，

  $$
  \varphi(1) = 1, \varphi(2) = 1, \varphi(5) = 4, \varphi(10) = 4

  $$

  把它们加起来，恰好结果是 $10$。

  证明：设 $n$ 的所有因子为 $d_1, d_2, \cdots, d_k$。对于每一个 $d_i$，与 $d_i$ 互质且小于 $d_i$ 的数为 $\beta_{i, 1}, \beta_{i,2}, \cdots, \beta_{i,\varphi(d_i)}$。

  定义集合 $A = \{ (\beta_{i,j}, d_i) \mid (\beta_{i,j}, d_i), i = 1,2,\cdots, k; j = 1, 2, \cdots, \varphi(d_i) \}$。显然， $\sum_{d \mid n} \varphi(d) = |A|$。

  定义集合 $B = \{1,2,\cdots, n \}$，即前 $n$ 个正整数的集合。若 $|A| = |B|$，则命题得证。下面证明集合 $A$ 和 $B$ 构成双射。

  定义映射

  $$
  (A \rightarrow B) : (\beta_{i,j}, d_i) \rightarrow \beta_{i,j} \cdot n / d_i

  $$

  这个映射是成立的，因为对于任意 $1 \le \beta_{i,j} \le d_i$，所以 $n/d_i \le \beta_{i,j} \cdot n / d_i \le n$，而且 $n / d_i$ 是整数，所以 $\beta_{i,j} \cdot n / d_i$ 必然是一个 $1$ 到 $n$ 的整数。

  先证明 $f$ 是单射。集合 $A$ 中的不同元素，有两种情况。

  - $i$ 相同但 $j$ 不同。设为 $(\beta_{i, j}, d_i)$ 和 $(\beta_{i, j'}, d_i)$， 其中 $j \ne j'$。这种情况下，显然  $\beta_{i, j} \ne  \beta_{i, j'}$，所以  $\beta_{i, j} \cdot n / d_i \ne  \beta_{i, j'} \cdot n / d_i$ ，所以对应集合 $B$ 中的元素是不相同的。
  - $i$ 不相同。设 $(\beta_{i, j}, d_i)$ 和 $(\beta_{i', j'}, d_i')$，$i \ne j$，显然 $d_i \ne d_{i'}$，此时无论 $j$ 是否相同，对应集合 $B$ 中的元素是不相同的。

    因为 $A$ 中对应 $B$ 中的元素是 $\beta_{i, j} \cdot n / d_i$ 和 $\beta_{i', j'} \cdot n / d_{i'}$。 若这二者相等，则有 $\beta_{i, j} \cdot d_{i'} = \beta_{i', j'} \cdot d_i$。因为 $\beta_{i,j}$ 与 $d_i$ 互质，所以要上式成立，必然有 $\beta_{i, j} \mid \beta_{i', j'}$；同理，因为 $\beta_{i',j'}$ 与 $d_{i'}$ 互质，等式成立，必然有 $\beta_{i', j'} \mid \beta_{i, j}$。这就是说，$\beta_{i, j} = \beta_{i', j'}$，这意味着 $d_i = d_{i'}$，因此与 $d_i \ne d_{i'}$ 矛盾。

  再证明 $f$ 是满射。对于 $B$ 中任意元素 $m$，令 $m$ 与 $n$ 的最大公约数是 $g$。那么显然 $m/g$ 与 $n/g$ 互质，且$m/g<n/g$。于是 $(m/g,n/g)$ 这个数对必然是集合 $A$ 中的一个元素，且按照映射 $f$，这个元素映射到 $B$ 中的元素就是$m$。

  于是，$B$ 中任意元素都会被 $A$ 中元素对应到。（满射得证）

  综上，映射 $f$ 是一个从集合 $A$ 到集合 $B$ 的一一对应，因此 $A$ 中元素个数与 $B$ 中元素个数相同。也即 $\sum_{d \mid n}\varphi(d) = n$。
