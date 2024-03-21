# 一、标题和正文

使用 **#** 表示标题， 其中 **#** 必须在行首，例如：
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

使用 === 或者 --- 表示，例如：

一级标题
===

二级标题
---

正文字体

1. 改变字体
在 Markdown 语法中，使用 <font> 标签的 face 属性修改文字字体，字体在不同环境中支持程度不同，表现结果可能也不同

<font face = "黑体">这里是黑体</font>
<font face = "楷体">这里是楷体</font>
<font face = "GB18030 Bitmap">这里是扩展字体</font>


<font face="黑体">我是黑体字</font> 
<font face="微软雅黑">我是微软雅黑</font> 
<font face="STCAIYUN">我是华文彩云</font> 
<font color=red>我是红色</font> 
<font color=#008000>我是绿色</font> 
<font color=Blue>我是蓝色</font> 
<font size=5>我是尺寸</font> 
<font face="黑体" color=green size=5>我是黑体，绿色，尺寸为5</font>

2. 修改字号
Markdown 有三种主要方式
第一种是使用<font>标签；
第二种通过'<big>'或者'<small>'标签；
第三种是通过修改<style>样式实现。 ^d0921d

<font size=1>1号字</font> 
<font size=3>3号字，默认</font> 
<font size=5>我是5号</font> 
<font size=7>7号，尺寸最大</font> 

3. 字体颜色
在 Markdown 语法中，使用'<font>' 标签的 color 属性修改文字颜色
<font color="red">红色</font>
<font color="green">绿色</font>
<font color="rgb(200, 100, 100)">使用 rgb 颜色值 </font>
<font color="#FF0088">使用十六进制颜色值</font>

设置字体背景色<style>
<table><tr><td bgcolor=yellow>背景色yellow</td></tr></table>

4. 上下标
H<sub>2</sub>O  CO<sub>2</sub>
爆米<sup>TM</sup>


# 二、分割线
连续三个* 或者-

***

---

# 三、粗体斜体
使用 '*' 或者'**' 分别表示斜体和粗体

*斜体*
**粗体**
***又斜又粗***

~~删除文字~~

# 四、超链接
Markdown 支持两种超链接的定义方式：行内定义 和 全局声明， 都是使用中括号[ ] 来声明

语法：中括号[链接名称](目标链接)

Obsidian简单入门教程[由此开始 Obsidian 使用教程 ① 入门介绍与Markdown详解_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV19a411s7R9/?spm_id_from=333.788&vd_source=ff081f9d2fd264d1cecbbcb4994bd540)

在Markdown 语法中，
语法： ![替换文字]（图片路径"标题(可选)")的形式定义图片

随便来一张
![随便一张图](https://pic4.zhimg.com/v2-5890f42834f834d3918afe9e33576b43_b.jpg)

# 五、有序和无序列表
1. 无序列表
使用 - 、+ 和 * 表示无序列表，前后留一行空白，可嵌套

使用星号生成无序列表
* 项目一

使用加号生成无序列表
+ 项目二

使用减号生成无序列表
- 项目三

无序列表的嵌套
在符号前面使用tab键将其缩进，每个tab表示一层
* 第一层 1
	* 第二层 1
		* 第三层 1
			* 第四层 1
				* 第五层 1
		* 第三层 2
	* 第二层 2

# 六、引用
使用 > 表示， 可以有多个 >, 表示层级更深，例如：

> 第一层
> > 第二层
> >> 还可以更深

# 七、 代码样式
1. 行内代码
使用[反引号  ``` ]符号定义行内代码 

'行内代码'

2. 代码块
使用四个空格缩进表示代码块，

```C
#include<stdio.h>
int main(void){
	printf("Hello world");
	}
```

```python
@requires_authorization
def somefunc(param1='', param2=0):
    '''A docstring'''
    if param1 > param2: # interesting
        print 'Greater'
    return (param2 - param1 + 1) or None

class SomeClass:
    pass

>>> message = '''interpreter
... prompt'''
```

```javascript
/**
* nth element in the fibonacci series.
* @param n >= 0
* @return the nth element, >= 0.
*/
function fib(n) {
  var a = 1, b = 1;
  var tmp;
  while (--n >= 0) {
    tmp = a;
    a += b;
    b = tmp;
  }
  return a;
}

document.write(fib(10));
```

# 八、表格
* **表头** 用来对列名对象进行描述，也就是通常所说的列名；
* **数据** 用来展示每行的具体内容，数据是表格的核心；
* **分割线** 用来区分表头和数据，也是 Markdown 中表格定义的最基本语法要求。

**Markdown** 表格由竖线|、减号-、冒号：三种符号组成。
* **竖线** 用来定义列，每两个竖线之间为一个单元格元素；
* **减号** 用来定义分割线，也就是分割表头和数据体；
* **冒号** 配合减号使用，用于定义列数据的对齐属性。

| 商品         | 数量 |  单价  |
| ------------ | ----:|:------:|
| 苹果苹果苹果 |   10 |   $1   |
| 电脑         |    1 | \$1999 |

# 九、数学公式
Markdown 中的数学公式支持 **LaTeX**，分为[行中公式]和[独立公式]两种。 ^20ca03

行中公式用两个单独的[美元符$]表示
世界上最难的问题$1+1=2$，如何证明？

独立公式用两个连续的[美元符$$]表示，换行通过 \ 实现
平均数符号： $$overlinexyz$$
开二次方符号： $$sqrtx$$
开方符号：$$sqrt[3]x+y$$
对数符号：$$log(x)$$
极限符号：$$lim^{x to infty}_{y to 0}{frac{x}{y}}$$

# 十、任务列表
在 Markdown 文件中，在“无序列表 -,+,* ”后面使用中括号声明复选框(里面要加个空格)。在中括号中写入x， 便可实现选中效果。

* [x] C++
* [ ] Java

# 十一、标签
#标签名字

# 十二、绘图
1. 流程图
```flow
st=>start: Start:>https://www.zybuluo.com
io=>inputoutput: verification
op=>operation: Your Operation
cond=>condition: Yes or No?
sub=>subroutine: Your Subroutine
e=>end

st->io->op->cond
cond(yes)->e
cond(no)->sub->io
```
2. 序列图
```seq
Title: Here is a title
A->B: Normal line
B-->C: Dashed line
C->>D: Open arrow
D-->>A: Dashed open arrow
```
3. 甘特图
```gantt
    title 项目开发流程
    section 项目确定
        需求分析       :a1, 2016-06-22, 3d
        可行性报告     :after a1, 5d
        概念验证       : 5d
    section 项目实施
        概要设计      :2016-07-05  , 5d
        详细设计      :2016-07-08, 10d
        编码          :2016-07-15, 10d
        测试          :2016-07-22, 5d
    section 发布验收
        发布: 2d
        验收: 3d
```

> [!info] > Here's a callout block. > It supports **Markdown**, [[Internal link|Wikilinks]], and [[Embed files|embeds]]! > ![[Engelbart.jpg]]

> [!note] > Here's a callout block. > It supports **Markdown**, [[Internal link|Wikilinks]], and [[Embed files|embeds]]! > ![[Engelbart.jpg]]

> [!tip] > Here's a callout block. > It supports **Markdown**, [[Internal link|Wikilinks]], and [[Embed files|embeds]]! > ![[Engelbart.jpg]]

> [!question] > Here's a callout block. > It supports **Markdown**, [[Internal link|Wikilinks]], and [[Embed files|embeds]]! > ![[Engelbart.jpg]]