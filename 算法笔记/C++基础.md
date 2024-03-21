从Hello World开始讲起
```c++
#include<iostream>

using namespace std;

int main()
{
    cout << "Hello World" << endl;
    
    return 0;
}
```

`<cstdio>`：只包含printf和scanf
`<iostream>`：包含两个重要函数: cin(输入) 和 cout(输出)

using namespace std：使用std命名空间（cin/cout均需要在这个命名空间才有定义)

**变量类型：**
1. **bool** : true, false （1 byte)
2. **char** 字符 'c', 'a' ' ', '\n' (单引号)(1 byte)
3. **int** 型范围:  $-2^{31}~2^{31}-1$（$-2147483648 ~ 2147483647$)(4 byte)
4. **float** 型:  单精度浮点数，精度在6-7位,1.23,2.5, 1.234e2(支持科学计数法) (8 byte)
5. **double** 型：15-16位有效数字 (8 byte)
6. **long long (int)** : $-2^{63} ~ 2^{63}-1$ (8byte)
7. **long double**:  18-19位有效数字 (16 byte)

**字节：**
B: Byte(字节)
b: bit(比特)
1Byte = 8 bit
1 Gb = $2^9$ b

cin/cou 使用(a+b)：
同样需要提前定义变量，想象成传送带，cin就传入，cout就是传出，注意最后需要endl
```c++
#include<iostream>

using namespace std;

int main()
{
    int a,b;
    
    cin >> a >> b;
    cout << a + b << endl;
    return 0;
}
```

在读入字符时，cin会过滤空格，scanf不会过滤空格，但是cin的速度会比较慢

int: %d
float: %f
double: %lf
char: %c
long long: %lld

数值运算和一般的一样，注意取余运算
在C中，5 % 2 = 1, -5 % 2 = -1, 但是因为余数只能为正，所以要对负数的余数进行转化 

各种数据转换

eg.int 转换 char 类型，即为寻找对应的ascii码
```c++
int t = 47;
char c = (char)t;

cout << c << endl; \\结果： /
```

反过来
```c++
char c = 'a';
int t = (int)c;

cout << t << endl; \\结果：97
```

