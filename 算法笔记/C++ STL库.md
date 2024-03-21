# 基本概念
STL(Standard Template Library, 标准模板库)
广义上讲分为三类： *algorithm(算法)、container(容器)和iterator(迭代器)*，容器和算法通过迭代器可以进行无缝地连接

几乎所有的代码都采用了模板类和模板函数的方式，相比于传统的由函数和类组成的库来说提供了更好的代码重用机会

C++标准中，STL被组织为13个头文件

**好处**：
	不用额外安装
	数据结构与算法分离 
	不需要思考STL具体的实现过程，只要能够熟练使用STL就可以
	STL具有高可重用性、高性能、高移植性，跨平台的特点

# 容器
## 分类
### 序列式容器(Sequence containers)
- 每个元素有固定位置——取决于插入时机和地点，和元素值无关
- `vector、deque、list、stack、queue`

### 关联式容器(Associated containers)
- 元素位置取决于特定的排序准则，和插入顺序无关
- `set、multiset、map、multimap`

| 数据结构 | 描述 | 实现头文件 |
| :--: | :--: | :--: |
| 向量 | 连续存储的元素 | vector |
| 列表 | 由节点组成的双向链表，每个节点包含着一个元素 | list |
| 双队列 | 连续存储的指向不同元素的指针所组成的数组 | deque |
| 集合 | 由节点组成的红黑树，每个节点都包含着一个元素，节点之间以某种作用于元素对的位次排列 | set |
| 多重集合 | 允许存在两个次序相等的元素的集合 | set |
| 栈 | 后进先出的值的排列 | stack |
| 队列 | 先进先出的值的排列 | queue |
| 优先队列 | 元素的次序是由作用于所存储的值对上的某种位次决定的一种队列 | queue |
| 映射 | 由键值对组成的集合，以某种作用域键对上的谓词排列 | map |
| 多重映射 | 允许键对有相等的次序的映射 | map |
`vector`容器与`deque`容器可以看作是动态数组，可以进行扩容，可以访问其下标，时间复杂度为$O(1)$，但缺点是在中间插入的话时间复杂度为$O(n)$
`list`链表查找/插入/删除的时间复杂度也为$O(n)$，但效率相对前两者高，通过改变前驱节点实现
![[微信截图_20231126125506.png|675]]

### vector容器
**变长数组**
#### 简介
- vector 是将元素置于一个动态数组中加以管理的容器
- 可以随机存取元素（支持索引值直接存取）
- 尾部添加或移除元素非常快捷，但在中部或头部插入元素或移除元素比较费时
#### 默认构造
采用模板类实现
```C++
vector<T> vecT;

vector<int> vecInt; //一个存放int的vector容器
vector<float> vecFloat; //一个存放float的vector容器
vector<string> vecString; //一个存放string的vector容器
... // 尖括号内还可以设置指针类型或者自定义类型
class CA{};
vector<CA*> vecpCA; //用于存放CA对象的指针的vector类型
vector<CA> vecCA; //用于存放CA对象的vector类型，由于容器元素的存放
                  //是安置复制方式进行的，所以此时CA必须提供CA的拷贝
                  //构造函数，以保证CA对象间拷贝正常
```

#### vector对象的带参数构造

**理论知识：**
- `vector(beg, end);` // 构造函数将\[beg, end)区间中的元素拷贝给本身。注意该区间是左闭右开的区间。
- `vector(n, elem);` // 构造函数将n个elem拷贝给本身
- `vector(const vector &vec);` //拷贝构造函数

```C++
#include <iostream>
#include <vector>

using namespace std;

int main(){
	int iarr[] = {1, 2, 3, 4, 5};
	vector<int> v1(iarr, iarr+5);
	
	vector<int> v2(3, 10); //存储3个10
	for (int i = 0; i < 3; i++)
		cout <<v2[i] << " ";
	cout << endl;
	/*也可以这么写*/
	for(vector<int>::iterator i = a.begin(); i < a.end(); i++) cout << i << ' ';
	// 等价于for(auto i = a.begin(); i < a.end(); i++)
	/*还可以这么写*/
	for(auto x : v2) cout << x << endl;
	
	vector<int> v3(v1);
	vector<int> v4 = v1;
	
	return 0;
}
```

#### vector的赋值
**理论知识：**
- `vector.assign(beg, end); `//将\[beg, end)区间中的数据拷贝赋值给本身。注意该区间是左闭右开的区间。(会清空原本容器的内容)
- `vector.assign(n, elem);` //将n个elem拷贝赋值给本身
- `vector& operator = (const vector &vec);` // 重载等号操作符
- `vector.swap(vec);` //将vec与本身的元素互换
- `vector.clear();` // 清空
```C++
vector<int> vecIntA(3, 10); 
vector<int> vecIntB, vecIntC, vecIntD;
int iArray[] = {0, 1, 2, 3, 4};

vecIntA.assign(iArray, iArray+5);
for (int i = 0 ; i < vecIntA.size(); i++)
	cout << vecIntA[i] << " ";
cout << endl; // 打印： 1 2 3 4 5

// 用其他容器的迭代器作参数
// 将vecIntA中的所有的元素赋值给vecIntB
vecIntB.assign(vecIntA.begin(), vecIntA.end()); 
// vector.begin()指向容器内的第0个元素； 
// vector.end()指向容器内最后一个元素的下一个元素

// 将4个10拷贝到vecIntC
vecIntC.assign(4, 10);

// 交换vector内容
vecIntB.swap(vecIntC);
for (int i = 0 ; i < vecIntB.size(); i++)
	cout << vecIntA[i] << " ";
cout << endl; // 打印： 10 10 10 10 
for (int i = 0 ; i < vecIntC.size(); i++)
	cout << vecIntA[i] << " ";
cout << endl; // 打印： 1 2 3 4 5 

// 赋值，即将vector的内容重载
vecIntD = vecIntC
```

#### vector的大小
**理论知识：**
- `vector.size();` // 返回容器中元素的个数
- `vector.empty();` // 判断容器是否为空
- `vector.resize(num);` // 重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。
- `vector.resize(num, elem);` 重新指定容器的长度为num， 若容器变长，则以elem值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除
```c++
// vector 大小
vector<int> v1; // 创造空向量
v1.empty() // 返回true

int iArray[] = {0, 1, 2, 3, 4};
vecIntA.assign(iArray, iArray+5);
v1.empty() // 返回false

// 将容器的长度变长
v1.resize(10);
for (int i = 0 ; i < vecIntA.size(); i++)
	cout << vecIntA[i] << " "; // 打印： 0 1 2 3 4 0 0 0 0 0 
// 变短同理

// 将容器的长度变长，并且扩展出来的新元素赋值为指定的值
v1.resize(10, 100); // 多填一个默认值
for (int i = 0 ; i < vecIntA.size(); i++)
	cout << vecIntA[i] << " "; // 打印： 0 1 2 3 4 100 100 100 100 100 
```
系统为某一程序分配空间时，所需时间与申请空间的大小无关，与申请次数有关
#### vector的访问方式
**理论知识：**
1. 常规的下标法: `vec[idx];` // 返回索引idx所指的数据，越界时，运行直接报错
2. `vec.at(idx);`  // 返回索引idx所指的数据，如果idx越界，抛出*out_of_range*异常
3. `vec.front/back();` // 返回第一个/最后一个值
4. 比较运算：按照字典序比较
```c++
vector<int> vecInt; // 假设包含1，3，5，7，9
vecInt.at(2) == vecInt[2]; // 5
vecInt.at(2) = 8; 或 vecInt[2]=8;
vecInt 就包含1，3，8，7，9值

int iF = vector.front(); // iF == 1
int iB = vector.back(); // iB == 9
vector.front() = 11; // vector:{11, 3 8, 7, 9}
vector.back() == 19; // vector:{11, 3, 8, 7, 19}

vector<int> a(4,3), b(3,4); // a为4个3，b为3个4
// a < b
```
#### vector的插入
**理论知识：**
1. vector的末尾**添加移除**操作
	- `vector.push_back();` // 在容器尾部加入一个元素
	- `vector.pop_back();` // 在容器尾部删除一个元素
```c++
int a[] = {1,2,3,4};
vector<int> v1(a, a + 4);

//
v1.push_back(10); // v1:{1,2,3,4,11}
v1.pop_back(); // v1:{1,2,3,4}
```

2. vector的**插入**
	- `vector.insert(pos, elem); `// 在pos位置插入一个elem元素的拷贝，返回新数据的位置
	- `vector.insert(pos, n, elem); `// 在pos位置插入n个elem数据，无返回值
	- `vector.insert(pos, beg, end);` // 在pos位置插入\[beg, end)区间的数据，无返回值
	- *注意*：第一个位置不能为下标，应该为指针
```c++
// 在指定的位置插入一个指定的元素
// v1:{1,2,3,4,10}
v1.insert(v1.begin() + 3, 100) //第一个参数为指针，不能为下标，这里为3的原因是100占了原本4的位置，所以这个地方对应的就为其指针
// v1:{1, 2, 3, 100, 4, 10}

// 在100的位置插入3个1000（在指定位置插入多个元素）
v1.insert(v1.begin()+3, 3, 1000); 

int b = [40, 50, 60, 70, 80, 90];
// 将指定的区间中的元素插入到指定的位置
v1.insert(v1.begin() + 7, b+1, b+5);

// 将容器数据进行插入
vector<int> v2(b, b+6);
v1.insert(v1.begin()+7, v2.begin()+1, v2.begin()+4); // 将容器v2中的50,60,70插入到v1中
// 注意普通数组和容器的差别
```

### 迭代器
#### 基本概念
一种检查容器内元素并且遍历容器内元素的**数据类型**
**作用：** 提供一个对一个容器中对象的范围方法，并且定义了容器中对象的范围
**使用原因：**
	STL提供每种容器的实现原理各不相同，如果没有迭代器我们需要记住每种容器中对象的访问方法，这样会变得非常麻烦
	每个容器中都实现了一个迭代器用于对容器中对象的访问，虽然每个容器的迭代器的实现方式不一样，但是对于用户来说操作方式是一致的，也就是说**通过迭代器统一了对容器的访问方式**。例如，无论对哪个容器，访问当前元素的下一个元素我们可以通过迭代器自增进行访问
	*迭代器是为了提高编程效率而开发的*

#### vector容器的iterator类型
- `vector<int> :: iterator iter;` // 变量名为iter
- vector容器的迭代器属于”随机访问迭代器“：迭代器一次可以移动多个位置
![[微信截图_20230818004423.png]]
- ++iter; // 使迭代器自增指向下一个元素
- \==和\!=操作符来比较两个迭代器，若两个迭代器指向同一个元素，则它们相等，否则不相等
##### begin和end函数
- 每种容器都定义了一对命名为begin和end的函数，用于返回迭代器。如果容器中有元素的话，由begin返回的元素指向第一个元素
- `vector<int>::iterator iter = v.begin();`// 若v不为空，iter指向v\[0]
- 由end返回的迭代器指向最后一个元素的下一个，若v为空，begin和end返回的相同
```c++
int iArray = {100, 1, 20, 30, 40};

vecIntA.assign(iArray, iArray+5);

// 构造一个迭代器对象
vector<int>::iterator it;
//让迭代器it执行 vetIntA 容器中的第一个元素
it = vecIntA.begin();

cout << *it <<endl; // 注意打印的时候由于it是迭代器，所以要加*，内部实现了*运算符的重载

// 通过循环的方式使用迭代器遍历vecIntA中的所有的元素
for(it = vecIntA.begin(); it != vecIntA.end() ;it++)
	cout << *it << " ";
cout << endl;

/*相当于*/
for(i = 0; i < vecIntA.size() ;it++)
	cout << vecIntA[i] << " ";
// 通过偏移n个位置实现访问

for(auto x : vecIntA) cout << x << ' ';

for(auto i = vecIntA.begin(); ...;...)
// auto可以让系统自己判断类型

it = vecIntA.begin();
it = it + 2;
cout << *it << endl;
```
#### 迭代器失效
**插入元素失效：**
```c++
vector<int> v;
v.push_back(1);
v.push_back(2);
v.push_back(3);
v.push_back(4);

vector<int>::iterator it = v.begin() + 3;
cout << *it << endl; // 输出 4

v.insert(it, 8);
/*
for(it2 = v.begin(); it2 != v.end(); it2++)
	cout << *it2 << " "; // 打印 1 2 3 8 4
cout << endl;
*/

// 但是打印这个迭代器的时候，出现了异常
cout << *it << endl; // 理论上应该输出8， 但实际却输出 0
```

这里的原因是原本的vector空间中只有 1, 2, 3, 4 这四个元素，此时it指向的是该位置的4，但由于发生了插入，4要向后移一位，容器扩容，但又因为后面一位的空间可能不能访问，所以vector*重新创造了一片含有五个元素的空间*（深拷贝），原来位置被释放了。但因为*it指向的是原来位置*，没有随之更改，就相当于变成了一个野指针，所以打印出来的可能是原来的内容，也有可能是释放内存后的内容

优化：
```c++
it = v.insert(it, 8); \\ insert会返回一个有效的迭代器
```

**删除元素失效：**
```c++
vector<int> cont = {1, 2, 3, 3, 3, 3, 4, 5, 5, 5, 6}
vector<int>::iterator iter;
for(iter = cont.begin(); iter != cont.end(); iter++){
	if(*iter == 3)
		cont.erase(iter);
}

for(iter = cont.begin(); iter != cont.end(); iter++)
	cout << *it << " "; // 打印： 1 2 3 3 4 5 5 5 6， 
	                    //        发现只删了一半的3
cout << endl;
```

删除的原理：删除该元素以后将后面的元素提前
![[vector迭代器删除方法.excalidraw]]

优化：
```c++
for(iter = cont.begin(); iter != cont.end(); ){
	if(*iter == 3)
		cont.erase(iter);
	else:
		iter ++;
}
```

erase也会返回一个迭代器
```c++
for(iter = cont.begin(); iter != cont.end(); ){
	if(*iter == 3)
		vector<int>::iterator it1;
		it1 = cont.erase(iter);
		cout << it1 <<endl; // 输出 3 3 3 4
	else:
		iter ++;
}
```

另一个优化方法：
```c++
for(iter = cont.begin(); iter != cont.end(); ){
	if(*iter == 3)
		iter = cont.erase(iter); // 最保守，更新迭代器，删除iter对应元素
	else 
		iter++;
}
```

### pair用法
存储一个二元组
- `first`第一个元素
- `second`第二个元素
- 支持比较运算，以`first`为第一关键字，以`second`为第二关键字
- 可以嵌套
```c++
pair<int, String> p;

p = make_pair(10, "yxc");  // 创建pair
p = (20, "abc");  // 

pair<int , pair<int, int>> p; // 嵌套
```

### deque容器
**简介：**
- deque是"double-ended queue"的缩写
- deque是**双端数组**，而vector是单端的（只能向一端移动）
- deque在接口上和vector非常相似，在许多操作的地方可以直接替换
- deque可以**随机存取元素**（支持索引值直接存取，用`[]`操作符或`at()`方法）
- deque头部和尾部添加或移除元素都非常快速，但是在中部安插元素或移除元素比较费时
- 由于速度慢，一般常用vector
- `# include <deque>`

**操作：**
- deque与vector在操作上几乎一样，deque多两个函数
- `deque.push_front(elem);` // 在容器头部插入一个数据
- `deque.push_back(elem)`
- `deque.pop_front(); `// 删除容器第一个数据
- `deque.pop_back(elem)`
- `d.front();` // 返回第一个元素
- `d.back();` // 返回最后一个元素
```c++
#include<iostream>
#include<deque>

using namespace std;

int main(){
	deque<int> d = {10, 20, 30, 40};

	d.front(); // 10, 返回第一个元素
	d.back(); // 40，返回最后一个元素
	
	// 在头部插入一个元素
	d.push_front(100);
	deque<int> :: iterator it;
	for(it = d.begin(); it != d.end(); it++)
		cout << it << " "; // 打印：100 10 20 30 40
	cout << endl;

	// 删除头部的元素
	d.pop_front();
	for(it = d.begin(); it != d.end(); it++)
		cout << it << " "; // 打印：10 20 30 40
	cout << endl;

	d.erase(d.begin());
	for(it = d.begin(); it != d.end(); it++)
		cout << it << " "; // 打印：20 30 40
	cout << endl；
	
	return 0;
}
```

vector与deque的\[]重载方式不一样，deque的存储的数据地址不一定连续，可能会是断裂的。但考虑效率，所以一般常用vector

### list容器
**简介：**
- list是一个双向列表容器，可高效地进行插入删除元素
- list不可以随机存取元素，所以不支持at.(pos)函数和\[]操作符
- It ++(ok); lt +5(err)
- \# include\<list>
- 链表可遍历，访问后不会被移除，栈和队列不可遍历

#### list构造：
采用模板类实现，对象的默认构建形式：list\<T> lst
```c++
#include<iostream>
#include<deque>

using namespace std;

int main(){
	list<int> lst;

	lst.push_back(10);
	lst.push_front(20);
	
	// 构造一个迭代器
	list<int>::iterator it;
	for(it = lst.begin(); it != lst.end(); it ++)
		cout <<it << " "; // 显示20 30 40
	cout << endl;
	
	return 0；
}
```

**头尾添加移除操作**
`list.push_back(elem); `// 在容器尾部加入一个元素
`list.pop_back(); `// 在容器开头加入一个元素
`list.push_frone(elem);` // 在容器尾部加入一个元素
`list.pop_front(); `// 在容器开头加入一个元素

#### list的数据提取
- `list.front();` // 返回第一个元素（头插法）
- `list.back(); `// 返回最后一个元素（尾方法）
```c++
// 返回第一个元素
int x = lst.front();
cout << "front:" << x <<endl;

x = lst.front();
cout << "front:" << x <<endl; // 返回值均为 20

// 返回最后一个元素
int x = lst.back();
cout << "back:" << x <<endl; // 返回值： 10

// 可以对容器元素(对象)进行修改
// 将第一个元素赋值为100，最后一个为200
lst.front() = 100;
lst.back() = 200;
```

#### list容器与迭代器：
- list 容器的迭代器是“双向迭代器”：双向迭代器从两个方向读写容器。除了提供前向迭代器的全部操作之外，双向迭代器还提供前置和后置的自减运算
- `list.begin();` //  返回容器中第一个元素的迭代器
- `list.end();` // 返回容器中最后一个元素之后的迭代器
- `list.begin(); `//  返回容器中倒数第一个元素的迭代器
- `list.end();` // 返回容器中倒数最后一个元素后面（第一个元素前面）的迭代器

```c++
list<int> lst;

lst.push_back(10);
lst.push_front(20);
lst.push_back(30);
lst.push_back(40);

// list容器的反向迭代器
list<int>::reverse_iterator it1;
for(it1 = lst.rbegin(); it1 != lst.rend(); it1++)
	cout << *it1 << " ";
cout << endl; // 打印 40 30 10 20
```

#### list 对象的带参数构造
- `list(n, elem);` // 构造函数将n个elem
- `list(beg, end);` // 构造函数将\[beg, end)区间中的元素拷贝给自身(这个地方是左闭右开)
- `list(consi list &lst); `// 拷贝构造函数
```c++
list<int> lst1(3, 5);
list<int>::iterator it2;

for(it2 = lst1.begin(); it2 != lst1.end(); it2++)
	cout << *it2 << " "; // 输出 5 5 5
cout << endl; 

// 注意list迭代器不允许自增操作，所以想要获得list某个区间内元素拷贝和vector不一样

// 此处lst 为{20 100 30 40}
list<int>::iterator beg = lst.begin();
beg++;
list<int>::iterator end = lst.begin();
end++; // 这里不允许直接end += 3; 因为list不支持此类操作，只能自增或自减
end++;
end++;
list<int> lst2(beg, end)

list<int>:: iterator it2;
for(it2 = lst1.begin(); it2 != lst1.end(); it2++)
	cout << *it2 << " "; // 输出 10 30 
cout << endl; 

list<int> lst3(lst);
list<int>:: iterator it2;
for(it2 = lst3.begin(); it2 != lst3.end(); it2++)
	cout << *it2 << " "; // 输出 20 10 30 40 
cout << endl; 
```

#### list 的赋值
- `list.assign(beg,end);` // 将\[beg,end)区间中的数据拷贝赋值给本身，注意该区间左闭右开
- `list.assign(n, elem);` //将n个elem拷贝赋值给本身
- `list& operator = (const list &lst);` // 重载等号操作符
- `list.swap(lst); `//将lst与本身的元素互换

```c++
list<int> lst4, lst5;
list<int>::iterator it3 = lst.end();
it3--;
lst4.assign(lst.begin(), it3);

for(it2 = lst4.begin(); it2 != lst4.end(); it2++)
	cout << *it2 << " "; // 输出 10 30  

lst4.assign(3, 6); // 会覆盖原本的内容，不是追加
for(it2 = lst4.begin(); it2 != lst4.end(); it2++)
	cout << *it2 << " "; // 输出 6 6 6 6

lst5 = lst4; // 10 30

lst5.assign(4, 6);
lst4.swap(lst5);

lst4.assign(3, 6); // 会覆盖原本的内容，不是追加
for(it2 = lst4.begin(); it2 != lst4.end(); it2++)
	cout << *it2 << " "; // 输出 6 6 6 6 

for(it2 = lst5.begin(); it2 != lst5.end(); it2++)
	cout << *it2 << " "; // 输出 10 30  
```

#### list大小
- `list.size();` // 返回容器中元素的个数
- `list.empty();` // 判断容器是否为空
- `list.resize(num);` // 重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。
- `list.resize(num, elem);` 重新指定容器的长度为num， 若容器变长，则以elem值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除

```c++
cout << "lst5.size:" << lst5.size() << endl; // 输出2
cout << "empty:" << lst5.empty() << endl; // 输出0

// 改变容器大小
lst5.resize(list5.size() + 3);
lst5.resize(5);
for (it2 = lst5.begin(); it2 != lst5.end(); it2++)
	cout << *it2* << " "; // 打印 10 30 0 0 0 
cout << endl;
// 变短同理

// 将容器的长度变长，并且扩展出来的新元素赋值为指定的值
lst5.resize(10, 5); // 多填一个默认值
for (it2 = lst5.begin(); it2 != lst5.end(); it2++)
	cout << *it2* << " "; //打印10 30 100 100 100
cout << endl;
```

#### list插入
- `list.insert(pos, elem);` // 在pos位置插入一个elem元素的拷贝，返回新数据的位置
- `list.insert(pos, n, elem);` // 在pos位置插入n个elem数据，无返回值
- `list.insert(pos, beg, end);` // 在pos位置插入\[beg, end)区间的数据，无返回值
- pos位置要求为迭代器实现

```c++
// lst5: [20 10 30]
list<int>::iterator it4 = lts5.begin();
it4++;
lst5.insert(it4, 40); // 在list容器的第2个节点的位置插入一个40
for (it2 = lst5.begin(); it2 != lst5.end(); it2++)
	cout << *it2* << " "; //打印20 40 10 30 
cout << endl;

// 在30的位置插入50
for(it4 = lst5.begin(); it4 != lst5.end(); it4++){
	if(it4 == 30)
		break;
}
if(it4 != lst5.end()){ // 找到了30
	lst5.insert(it4, 50);
	cout << "it4:" << it4 << endl; // 打印: it4: 30
}
for (it2 = lst5.begin(); it2 != lst5.end(); it2++)
	cout << *it2* << " "; //打印20 40 10 50 30 
cout << endl;

// 错误示例
lst5.insert(3, 100); // 没有这种方法
```

注意，插入元素不会导致迭代器失效，因为其实现原理不同，插入过程没有空间的释放，位置也没有变动，只是改变了前驱指针和后驱指针

#### list删除
- `list.clear();` // 移除容器中所有数据
- `list.erase(beg, end); `// 删除\[beg, end)区间的数据，返回下一个数据的位置
- `list.erase(pos); `// 删除pos位置的数据，返回下一个数据的位置
- `list.remove(elem);` // 删除容器中所有与elem值匹配的元素
```c++
// lst5:20 40 50 30 10 
// list容器的删除
it4 = lst5.begin();
it4++;
it4++;
it4 = lst5.erase(it4);
for(it2 = lst5.begin(); it2 != lst5.end(); it2++)
	cout << *it2 << " "; // 输出 20 40 30 10 
cout << endl;

cout << "*it4:" << *it4 << endl; //it4: 30

// 删除某个区间的元素
list<int>::iterator it5 = it4;
it5++; it5++;
it5.erase(it4, it5);
for(it2 = lst5.begin(); it2 != lst5.end(); it2++)
	cout << *it2 << " "; // 输出 20 40 
cout << endl;

// 删除所有的20
lst5.push_fornt(20);
lst5.remove(20);
for(it2 = lst5.begin(); it2 != lst5.end(); it2++)
	cout << *it2 << " "; // 输出 40 
cout << endl;

// 清空容器
lst5.clear(); 
cout << "empty?:" << lst5.empty() << endl; // 输出empty ?: 1
```

#### list反转
- `list.reverse();` // 反转链表
```c++
list<int> lstA;
lstA.push_back(1);
lstA.push_back(3);
lstA.push_back(5);
lstA.push_back(7);

lstA.reverse();
list<int>::iterator it2;
for(it2 = lstA.begin(); it2 != lstA.end(); it2++)
	cout << *it2 << " "; // 7 5 3 1 
cout<< endl;
```

#### list迭代器失效
- 删除节点导致迭代器失效
```c++
for(list<int>::iterator it = lstInt.begin(); it != istInt.end(); ){//小括号不需要写 it++
	if(*it == 3)
		lstInt.erase(it); // 以迭代器为参数，删除元素3，并把数据删除后的下一个元素位置返回给迭代器
	else
		it++;
}
```

### stack容器
#### stack简介
- `stack`是一种堆栈容器，是一种“先进后出”的容器
- `#include<stack>`
#### stack默认构造
- `stack`采用模板类实现，`stack`对象的默认构造形式 `stack<T> s`;
- `stack<int> stkInt;` // 存放int的stack容器，float、string等同理；
#### stack的push与pop方法
由于stack先进后出的原因，只能对栈顶进行添加与删除元素
```c++
stack.push(elem); // 往栈头添加元素
stack.pop(); // 往栈头移除第一个元素

stack<int> stkInt;
stkInt.push(1); stkInt.push(3); stkInt.pop();
stkInt.push(5); stkInt.push(7);
stkInt.push(9); stkInt.pop();
stkInt.pop(); // 此时stkInt存放的元素是1, 5
```
#### 访问stack元素
`stack.top();` // 返回栈顶第一个元素，不会删除栈顶元素
```c++
cout << stkInt.top() << endl; // 5
stkInt.pop(); 
cout << stkInt.top() << endl; // 1
```
#### stack赋值
- `stack<int> stack = stk;` // 调用拷贝构造函数
- `stack& operator = (const stack &stk);` // 重载等号操作符
#### stack大小
- `stack.size();` // 返回stack大小
- stack没有`resize方法`
```c++
cout << stk.size() << endl; 
```
**stack没有迭代器**

### queue容器
#### 简介
- queue是队列容器，是一种“**先进先出”** 的容器
#### 默认构造
- 采用模板类实现： `queue <T> q;`
```c++
queue<int> queInt;
```
#### push与pop方法
- `queue.push(elem);` // 往队尾添加元素
- `queue.pop();` // 从队头移除第一个元素
```c++
queue<int> queInt;
queInt.push(1); queInt.push(3); queInt.pop();
queInt.push(5); queInt.push(7);
queInt.push(9); queInt.pop();
queInt.pop(); // 此时stkInt存放的元素是5, 7, 9
```
#### queue容器对象的拷贝构造与赋值
- `queue(const queue & que);` // 拷贝构造函数
- `queue& operator = (const queue & que); `// 重载等号运算符
```c++
queue<int> queIntB(queInt); // 拷贝构造
queue<int> queIntC;
queIntC = queInt; // 赋值
```
#### queue容器数据存取
- `queue.back();` // 返回最后一个元素
- `queue.front()`; // 返回第一个元素
也可以用这两个函数赋值
```c++
int iFront = queInt.front(); // 5
int iBack = queInt.back(); // 9

queue.front() = 11; // 赋值为11
queue.back() = 19; //赋值为19
```
#### queue容器大小
- `queue.empty();` // 判断队列是否为空
- `queue.size(); `// 判断队列大小

**queue**没有`cleaar()`函数

### 优先队列 priority_queue
**默认大根堆**
- `push()`
- `pop()`
- `top()`
```c++
priority_queue<int> heap;

// 构造小根堆的方法
// 第一种，直接插入-x
heap.push(-x); // 插入负数，则反过来就是正数从小到大

// 另一种构造方法
priority_queue<int, vector<int>, greater<int>> heap;


// 含有其他元素的构造
struct Compare {  
    bool operator()(const pair<int, string>& lhs, const pair<int, string>& rhs) {  
        return lhs.first > rhs.first; // 根据整数进行排序  
    }
};

priority_queue<pair<int, string>, vector<pair<int, string>>, Compare> q;  
```

### set容器
#### 简介
- `set`是一个集合容器，其中所包含的**元素是唯一**的，集合中的元素按一定的顺序排列。元素插入过程是按排序规则插入，所以不能指定插入位置
- `set`采用红黑树变体的数据结构实现，红黑树属于平衡二叉树，在插入操作和删除操作上比`vector`快。
- set*不能直接存取元素*（不能使用`at.(pos) `和`[]`操作符）
- `multiset`和`set`区别：`set`支持**唯一键值**，每个元素值只能出现一次；而`multiset`中**一个值可出现多次**。
- 不可以直接修改`set`和`multiset`容器中的元素值，因为该类容器是自动排序的。如果希望修改一个元素，需要先删除原有的元素，再插入新的元素

#### set容器插入与迭代器
- `set.insert(elem);` // 在容器中插入元素
- `set.begin(); `// 返回容器中第一个元素的迭代器
- `set.end(); `// 返回容器中最后一个元素的迭代器
- `set.rbegin(); `// 返回容器中倒数第一个元素的迭代器
- `set.rend();` // 返回容器中倒数最后一个元素的迭代器
```c++
set<int>s1; 
s1.insert(3);
s1.insert(1);
s1.insert(7);
s1.insert(5);

set<int>::iterator it;
for(it = s1.begin(); it != s1.end(); it++)
	cout << *it << " "; // 1 3 5 7， 发现已经排好
cout << endl;

set<int>::reverse iterator it2;
for(it2 = s1.rbegin(); it2 != s1.rend(); it2++)
	cout << *it2 << " "; // 1 3 5 7， 发现已经排好
cout << endl;

```
#### set/multiset容器对象的默认构造
```c++
set<int> setInt;
multiset<string> multiString;
...
```
#### set容器的大小
- `set.size(); `
- `set.empty();`
#### set容器的删除
- `set.clear();` //清除所有元素
- `set.erase();` // 删除pos迭代器所指的元素，返回下一个元素的迭代器
- `set.erase(beg, end); `// 删除区间\[beg.end)的元素
- `set.erase(elem);` // 删除容器中值为elem的元素
```c++
// setInt: 1, 3, 5, 7, 9, 11
set<int>iterator itBegin = setInt.begin();
++itBegin;
set<int>::iterator itEnd = setInt.begin();
++itEnd;
++itEnd;
++itEnd;
setInt.erase(itBegin, itEnd); // 此时容器包含按顺序的1，7，9，11

// 删除容器中第一个元素
setInt.erase(setInt.begin()); // 6 9 11

// 注意set容器用反向迭代器对元素进行删除
// set<int>reverse iterator:: it3 = s3.rbegin(); 
// s3.erase(it3); // 会报错
// set删除最后一个元素
it = s3.end();
it--;
it = s3.erase(it);
it++; // 迭代器重新指向set容器中第一个元素
cout << "it: " << *it << endl; // 返回set容器第一个数

// 删除容器中值为9的元素
set.erase(9);

// 如果需要删除的元素不在set中：返回false；反之返回true

// 删除setInt的所有元素
setInt.clear(); // 容器为空
```

#### set容器的元素排序
- `set<int, less<int>> setIntA;` //  升序排列
- `set<int, greater<int>> setIntB;` //  降序排列
```c++
set<int, less<int>> s1;
set<int, greater<int>> s2;

s1.insert(1);
s1.insert(5);
s1.insert(3);

s2.insert(1);
s2.insert(5);
s2.insert(3);
```
*less<> 与greater<>是模板类*
如果`set<>`不包含int类型，而是包含自定义类型，set容器如何排序？

*函数对象function的用法*
- 尽管函数指针被广泛用于实现函数回调，但C++还提供了一个重要的实现回调函数的方法，那就是函数对象
- functor，翻译成函数对象，伪函数，算符，是重载了"()"操作符的普通类对象，从语法上来讲，它与普通函数行为类似
- `greater<>`与`less<>`就是函数对象，制定排序规则

*简易实现原理（实际有些不同）：*
```c++
class greater{
	bool operator()(const int& iLeft, const int& iRight){
		return (iLeft > iRight); // 如果是实现less<int>的话，就写return (iLeft < iRight)
	}
}
```

函数对象举例：
```c++
typedef int (*FUNC)(int , int ); // FUNC即为函数指针

int func1(int x, int y){
	cout << "func1" << endl;
	return x + y;
}
int func2(int x, int y){
	cout << "func2" << endl;
	return x - y;
}
int func3(int x, int y){
	cout << "func3" << endl;
	return x * y;
}
int func4(int x, int y){
	cout << "func4" << endl;
	return x / y;
}
int func(int x, int y, FUNC f){
	return f(x, y);
}

cout << func(10, 20, func1) << endl; // 30 
```

**容器就是调用函数对象的operator()方法去比较两个值的大小**

eg. 学生学号排序
```c++
//学生类
class CStudent{
public:
	CStudent(int iID, string strName){
		m_iID = iID;
		m_strName = strName;	
	}
	int m_iID; // 学号
	string m_strName; // 姓名
}

// 伪函数
class StuFunctor{
public:
	bool operator()(const CStudent &s1, const CStudent &s2){
		return s1.m_iID < s2.m_iID;
	}
}

// 定义一个容器实现存储学生信息，按照StuFunctor
set<CStudent, StuFunctor> stu;
stu.insert(CStudent(10, "xiaoming"));
stu.insert(CStudent(8, "xiaowang"));
stu.insert(CStudent(6, "xiaoma"));
stu.insert(CStudent(12, "xiaozhou"));

set<CStudent, StuFunctor>:: iterator it1;
for(it1 = stu.begin(); it1 != stu.end(); it1++)
	cout << it1 -> m_iID << " "; //6 8 10 12
cout << endl; 
```

#### set容器的查找
- `set.find(elem); `//查找elem元素，返回指向elem元素的迭代器，不存在则返回`end()`迭代器
- `set.count(elem);` // 返回容器中值为elem的元素个数。对set来说，要么是0，要么是1。对multiset来说，值可能大于1 // 可判断某个元素是否在容器内
- `set.lower_bound(elem);` // 返回第一个`>=elem`元素的迭代器
- `set.upper_bound(elem);` // 返回第一个`>elem`元素的迭代器
```c++
//s1: 1 3 5 
set<int>::iterator it;
it = s1.find(3); // 返回迭代器，找不到不会返回NULL，而会返回end()
cout << *it << endl; // 3

it = s1.find(10);
if(it == s11.end())
	cout << "can not find 10 " << endl;
```
```c++
cout << s1.count(1) << endl;
count << s1.coun(10) << endl;
```
```c++
// 查找第一个大于等于6的元素

it = s1.lower.bound(6);
cout << ">=6" << *it << endl;
```

// 查找第一个大于6的元素
```c++
it = s1.upper_bound(6);
cout << "6 :" << *it << endl;
```

#### set容器`set.equal range(elem)`
- 返回容器中与elem相等的上下限的两个迭代器。上限是闭区间，下限是开区间
- 函数返回两个迭代器，而这两个迭代器被封装在pair中
```c++
// setInt: 1 3 5 6 7 8 10
pair<set <int>::iterator, set<int> ::iterator> pairIt = setInt.equal_range(5);
cout << *(pairIt.first) << endl; // first是pair对组中的第一个元素，此处返回5
cout << *(pairIt.second) << endl; // first是pair对组中的第二个元素，此处返回6

```

### map容器
#### 默认构造
- `map/multimap`采用模板类实现，对象的默认构造形式：`map<T1, T2> mapTT;`
```c++
class Student
{
public； 
	int id;
	string name;

	Student()
	{}

	Student(int id, string name)
	{this -> id = id; this -> name = name;
	}

	friend ostream &operator << (ostream &out, Student &t)
	{
		out << t.name;
		return out;
	}
};

int main(){
	//构造一个map容器对象，存储student对象
	map<int, Student> stus;
	Student s1(1, "xiaoli");
	stus.insert(pair<int, Student>(s1.id, s1)); 
	
	Student s2(2, "xiaowang");
	stus.insert(map<int, Student> :: value_type(s2.id, s2));
	
	Student s3(3, "xiaotian");
	
}
```

#### map容器插入
- `map.insert(); `// 插入元素(pair)，返回pair
- 插入元素的三种方式：
	- 通过pair方式插入对象
	- 通过value_type的方式插入对象
	- 通过数组的方式插入值(对中括号进行重载)
```c++
map<int, string> mapS;
mapS.insert(pair<int, string>(3, "xiaozhang")); // 3是键，xiaozhang是值 

mapS.insert(map<int, string>::value_type(1, "xiaoli"));
map<int, string>::value_type v(6, "xiaozhou");
mapS.insert(v);

mapS[2] = "xiaoliu";

int key;
string name;
key = 5;
name = "xiaozhao";
pair<int, string> p(key, name);
mapS.insert(p);
```
- 第三种方法非常直观，但存在一个性能的问题。插入3时，现在map中寻找主键为key的项，若没发现，则将一个键为key，值为value的键值对插入到map中。若发现已存在key这个键，则修改这个键对应的value
- 如果键存在则修改，如果不存在则插入
- 第三种方法实现逻辑：①插入；②修改
```c++
mapS.insert(pair<int, string>(6, "xiaoliang")); // 使用insert方法不会覆盖原来的键值对

mapS[2] = "xiaoma"; // 用类似数组的方式插入会覆盖原来的键值对

// 获取学号为3的学生名字
string stu_name = mapS[3];
cout << stu_name << endl;

// 获取学号为10的学生名字
string stu_name = mapS[10];
cout << stu_name << endl; // 输出空内容
// 如果容器中不存在键为10的键值对，自动创建一个键为10的键值对，值是默认值""
```
#### map容器迭代
```c++
for(map<int, string>::iterator it = mapS.begin(); it != mapS.end(); it++){
	pair<int, string> p = *it1;
	int key = p.first;
	string value = p.second;

	cout << key << ":" << value <<endl;
	//打印匹配的键值对
}

map<int, Student>::iterator it2;
for(map<int, Student>::iterator it2 = stus.begin(); it2 != stus.end(); it++){
	pair<int, string> p = *it2;
	int key = p.first;
	Student value = p.second;

	cout << key << ":" << value.name <<endl;
	//打印匹配的键值对
	// 由于在Student类中实现了输出运算符的重载，所以也可以这么写
	cout << key << ":" << value << endl;
}
```
#### map容器对象获取键对应的值
- 使用`[]` （最好，但时间复杂度是$O(logn)$）
- 使用`find()`函数：成功返回对应的迭代器，失败返回end()的返回值 
- 使用`at()`函数，如果键值对不存在会抛出"out_of_range 异常"
```c++
map<int, string> :: iterator it3;
it3 = mapS.find(3);
if(it3 != mapS.end())
	cout << "3:" << (*it3).second << endl;
else
	cout << "can not find key:10" << endl;

it3 = mapS.find(10);
if(it3 == mapS.end())
	cout << "can not find key:10" << endl;

cout << mapS[3] << endl; // 键值对匹配
cout << mapS.at(3) << endl; // 正常输出
cout << mapS.at(20) << endl; // 抛出异常
```
#### 删除erase()

### unordered_map
- 增删改查时间复杂度都为$O(1)$
- 一个将`key和value`关联起来的容器，它可以高效的根据单个key值查找对应的value
- key值应该是唯一的，key和value的数据类型可以不相同
- `unordered_map`存储元素时是没有顺序的，只是根据key的哈希值，将元素存在指定位置，所以根据key查找单个value时非常高效，平均可以在常数时间内完成。
- `unordered_map`查询单个key的时候效率比map高，但是要查询某一范围内的key值时比map效率低
- 可以使用\[\]操作符来访问key值对应的value值
- 不支持迭代器++，--，以及`lower_bound()`和`upper_bound()`操作

***简单使用:***
```c++
std::unordered_map<std::string, std::int> umap; //定义

umap.insert(Map::value_type("test", 1));//增加

//根据key删除,如果没找到n=0
auto n = umap.erase("test")   //删除

auto it = umap.find(key) //改
if(it != umap.end()) 
    it->second = new_value; 


//map中查找x是否存在
umap.find(x) != map.end()//查
//或者
umap.count(x) != 0

```
**注意：使用auto循环时候，修改的值作用域仅仅循环之内，出去循环还会变成未修改的数值。**

```c++
#include <iostream>
#include <unordered_map>
using namespace std;
int main()
{
	string key="123";
	int value=4;
	unordered_map<string, int> unomap;//创建一个key为string类型，value为int类型的unordered_map
	unomap.emplace(key, value);//使用变量方式，插入一个元素
	unomap.emplace("456", 7);//也可以直接写上key和value的值
	cout << unomap["123"];//通过key值来访问value

	cout<<endl;
	for(auto x:unomap)//遍历整个map，输出key及其对应的value值
		cout<<x.first<<"  "<<x.second<<endl;

	for(auto x:unomap)//遍历整个map，并根据其key值，查看对应的value值
		cout << unomap[x.first]<<endl;
}
```

### bitset
- 期望实现压位
- 原本是1024个状态，需要1024个字节，但现在每个状态只需要一位字节，1024个状态只需要128个字节(用1位代替1字节存储)
- 支持位运算的一切操作
假设需要一个$10000 \times 10000$的布尔矩阵，如果直接用`bool`存储，则需要100MB，但`bitset`可以省八倍空间，即只要12MB
```c++
bitset<10000>s; 创建10000的数组
/*
	~, &, |, ^
    >>, <<
	==, !=
	[]
	
	count() 返回有多少个1
	any() 判断是否至少有个1
	none() 判断是否全为0
	set() 把所有位置成1
	set(k, v) 把第k位变成v
	reset() 把所有位置变为0
	flip() 等价于 ~
*/

```

### string 字符串
`substr(int a, int b)` 返回从a开始，长度为b的子串 
`c_str` 返回一个指向正规C字符串的指针常量
`size()/strlen()`
`clear()`

```c++
// string 支持添加元素
string a = "yxc";
a += "def";
a += "c";
cout << a << endl; // yxcdefc

// 返回子串 
cout << a.substr(1, 2) // xc

printf("%s\n", a.c_str()); // 返回起始地址，yxcdefc
```

