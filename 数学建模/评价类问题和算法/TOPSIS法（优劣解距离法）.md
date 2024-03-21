TOPSIS法 (Technique for Order Preference by Similarity to Ideal Solution，可翻译为逼近理想解排序法，国内常简称为优劣解距离法)，是一种常用的综合评价方法，其能充分利用原始数据的信息，其结果能精确反映各评价方案之间的差距。

基本过程为先将原始数据矩阵统一指标类型（一般**正向化处理**）得到正向化的矩阵，再对正向化的矩阵进行**标准化处理**以消除各指标量纲的影响，并找到有限方案中的**最优方案和最劣方案**，然后分别计算各评价对象与最优方案和最劣方案间的**距离**，获得各评价对象与最优方案的**相对接近程度**，以此作为评价优劣的依据。该方法对数据分布及样本含量没有严格限制，数据计算简单易行。

*很好反映问题的几张ppt：*
![[微信截图_20230704134426.png]]

想法一：根据排名赋权重![[微信截图_20230704134439.png]]

想法二：根据高低分差赋权重![[微信截图_20230704134503.png]]

想法三：根据占的百分比赋权重![[微信截图_20230704134513.png]]

想法三在实际上较难实施，原因：
 1. 比较的对象一般远大于两个（例如比较一个班级的成绩）
 2. 比较指标往往不只是一个方面的，例如成绩、工时数、课外竞赛得分等
 3. 有很多指标**不存在理论上的最大值与最小值**，如衡量经济增长水平的指标：GDP

构造计算评分的公式：$$\frac{x-min}{max-min}$$
**拓展：增加指标个数**
![[微信截图_20230704135448.png]]

## 统一指标类型
将所有的指标转化为极大型称为**指标正向化**（最常用）

| 姓名     |   成绩 | 与他人争吵次数 | 正向化后争吵次数 |
|: -------- :| :------:|:--------------:| :----------------: |
| 小明     |     89 |       2        | 1                |
| 小王     |     60 |       0        | 3                |
| 小张     |     74 |       1        | 2                |
| 清风     |     99 |       3        | 0                |
| 指标类型 | 极大型 |     极小型     | 极大型           |

极小型指标**转化**为极大型指标公式：$max - x$

## 标准化处理

为了**消去不同指标量纲的影响**，需要对已经正向化的矩阵进行**标准化处理**  ^d51a36

**计算公式：**
假设有 $n$ 个要评价的对象，$m$ 个评价指标（已经正向化）构成的正向化矩阵如下：  
$$X=\begin{equation}\begin{bmatrix}
x_{11} & x_{12}& \cdots & x_{1m} \\
x_{21} & x_{22}& \cdots & x_{2m} \\
\vdots & \vdots &\ddots & \vdots \\ 
x_{n1} & x_{n2} &\cdots & a_{nm} \end{bmatrix}\end{equation}$$
那么，对其标准化的矩阵记为 $Z$，$Z$ 中的每一个元素：$$z_{ij}=\frac{x_{ij}}{\sqrt{\sum_{i=1}^{n} x_{ij}^2}}$$

$\begin{equation}\begin{bmatrix}89 &1\\ 60 & 3\\ 74 & 2\\ 99 & 0 \end{bmatrix}\end{equation}$ 经过标准化后就变成了$\begin{equation}\begin{bmatrix}0.5437 &0.2673\\ 0.3665 & 0.8018\\ 0.4520 & 0.5345\\ 0.6048 & 0 \end{bmatrix}\end{equation}$

## 计算得分
*如何计算得分？* 公式变形
$$ \frac{x-min}{max-min} = \frac{x-min}{(max-x)+(x-min)} =\frac{x与最小值的距离}{x与最大值的距离+x与最小值的距离}$$

>为什么要这样变形呢？因为大家都是这么用的……好吧，其实我们接下来是使用**欧几里得距离**来衡量两个方案的距离，**变形前后分母的计算结果其实是不同的**。我个人认为这样变形更有利于说明问题，即我们衡量的得分是考虑到某个方案距离最优解和最劣解的一个**综合距离**。不然的话，所有方案计算得分时分母都是相同的，相当于只衡量了分子，也就是距离最劣解的距离。那应该还是采用综合衡量的方式会好一点儿吧，你觉得呢？(摘自知乎)

假设有 $n$ 个要评价的对象，$m$ 个评价指标（已经正向化）构成的标准化矩阵如下：
$$Z=\begin{equation}\begin{bmatrix}
z_{11} & z_{12}& \cdots & z_{1m} \\
z_{21} & z_{22}& \cdots & z_{2m} \\
\vdots & \vdots &\ddots & \vdots \\ 
z_{n1} & z_{n2} &\cdots & z_{nm} \end{bmatrix}\end{equation}$$
定义最大值$Z^+ = (Z_1^+, Z_2^+,..., Z_m^+) = (max\{z_{11},z_{21},...,z_{n1}\}, max\{z_{12},z_{22},...,z_{n2}\}, ... , max\{z_{1m},z_{2m},...,z_{nm}\})$
定义最小值$Z^- = (Z_1^-, Z_2^-,..., Z_m^-) = (min\{z_{11},z_{21},...,z_{n1}\}, min\{z_{12},z_{22},...,z_{n2}\}, ... , min\{z_{1m},z_{2m},...,z_{nm}\})$
定义第 $i(i=1,2,...,n)$ 个评价对象与最大值的距离$D_i^+ = \sqrt{\sum_{j=1}^{m}(Z_j^+-z_{ij})^2}$
定义第 $i(i=1,2,...,n)$ 个评价对象与最小值的距离$D_i^- = \sqrt{\sum_{j=1}^{m}(Z_j^--z_{ij})^2}$

那么，可以计算得出第 $i(i=1,2,...,n)$ 个评价对象未归一化的得分：$$S_i = \frac{D_i^-}{D_i^+ +D_i^-}$$
很明显$0 \leq S_i \leq 1$，且 $S_i$ 越大 $D_i$ 越小，即越接近最大值。

![[微信截图_20230704145457.png]]
![[微信截图_20230704145907.png]]


# TOPSIS法整体步骤
## Step1: 将原始矩阵正向化
![[微信截图_20230704150049.png]]

1. **极小型**转化为极大型
	$max - x$ ，如果所有元素均为正数，也可以使用$\frac{1}{x}$

2. **中间型**转化极大型（公式不唯一）
	中间型指标：指标值既不要太小也不要太大，取某特定值最好（如水质量评估PH值）
	$\{x_i\}$是一组中间型指标序列，且最佳的数值为$x_{best}$，那么正向化的公式如下：
$$M = max\{|x_i - x_{best}|\},\quad \hat{x_i} = 1-\frac{|x_i - x_{best}|}{M}$$
eg. ![[微信截图_20230704150756.png]]

3. **区间型**转化极大型
	区间型指标：指标值落在某个区间内最好，例如人的体温
	$\{x_i\}$是一组区间型指标序列，且最佳的区间为$[a,\ b]$，那么正向化的公式如下：
$$M = max\{a-min\{x_i\}\ ,max\{x_i\} - b\},\quad \hat{x_i} = \left\{\begin{aligned}&1-\frac{a-x_i}{M},\ &x_i<a \\&1 ,\ & a \leq x_i \leq b \\&1 - \frac{x_i-b}{M}, \ &x_i >b\end{aligned} \right.$$
![[微信截图_20230704151925.png]]

## Step2:  正向化矩阵标准化

为了**消去不同指标量纲的影响**，需要对已经正向化的矩阵进行**标准化处理**  

**计算公式：**
假设有 $n$ 个要评价的对象，$m$ 个评价指标（已经正向化）构成的正向化矩阵如下：  
$$X=\begin{equation}\begin{bmatrix}
x_{11} & x_{12}& \cdots & x_{1m} \\
x_{21} & x_{22}& \cdots & x_{2m} \\
\vdots & \vdots &\ddots & \vdots \\ 
x_{n1} & x_{n2} &\cdots & a_{nm} \end{bmatrix}\end{equation}$$
那么，对其标准化的矩阵记为 $Z$，$Z$ 中的每一个元素：$$z_{ij}=\frac{x_{ij}}{\sqrt{\sum_{i=1}^{n} x_{ij}^2}}$$
($每一个元素/\sqrt{其所在列的元素的平方和}$)

## Step3: 计算得分并归一化

注意：要区别开归一化和标准化。归一化的计算步骤也可以消去量纲的影响，但更多时候，我们进行归一化的目的是为了让我们的结果更容易解释，或者说让我们对结果有一个更加清晰直观的印象。例如将得分归一化后可限制在0‐1这个区间，对于区间内的每一个得分，我们很容易的得到其所处的比例位置。

![[TOPSIS法（优劣解距离法）#计算得分]]没有考虑指标的权重，后面的内容会考虑指标的权重来进行计算。

## 代码实现
### 基本的TOPSIS法
```python
import numpy as np  
  
# 定义正向化函数：  
# 极小型->极大型  
def positivization_1(X):  
    M = np.max(X)  
    # len(X)与X.shape(0)效果一样（二维数组情况下）  
    for i in range(0, len(X)):  
        X[i] = M-X[i]  
    return X  
  
# 中间型->极大型  
def positivization_2(X, best):  
    # 一个向量与一个常数做加减乘除，相当于对向量中的每一个数进行加减乘除  
    M = np.max(np.abs(X - best))  
    for i in range(0, len(X)):  
        X[i] = 1 - np.abs(X[i] - best)/M  
    return X  
  
# 区间型->极大型  
def positivization_3(X, a, b):  
    M = max(a - np.min(X), np.max(X) - b)  
    for i in range(0, len(X)):  
        if X[i] <= a:  
            X[i] = 1 - (a-X[i])/M  
        elif X[i] >= b:  
            X[i] = 1 - (X[i]-b)/M  
        else:  
            X[i] = 1  
    return X  
  
#定义标准化函数：  
def standardize(X):  
    '''  
    # 写法一：遍历每一列  
    for j in range(0, len(X[0])):        
	    square_sum = 0        
	    # 遍历每一行  
        for i in range(0, len(X)):            
	        square_sum = square_sum + X[i][j] ** 2        
	    for i in range(0, len(X)):            
		    X[i][j] = X[i][j] / np.sqrt(square_sum)    
	return X    
	'''  
    # 写法二：  
    # axis=0, 压缩行，即将每一列的元素相加，将矩阵压缩成一行  
    SUM = np.sum(X ** 2, axis=0)  
    # 将矩阵扩充为与原本大小相同  
    SUM = np.tile(SUM, (len(X[0]), 1))  
    Z = X / np.sqrt(SUM)  
    return Z  
  
# 定义打分函数：  
def grade(Z):  
    # 获得每一列的最大值/最小值，返回的是一个1xn的行向量（矩阵为mxn)  
    Z_max = np.max(Z, axis=0)  
    Z_min = np.min(Z, axis=0)  
  
  
    # 方法一：转置后对每一列（转置后变成行）循环  
    # 这个地方逻辑可能有点绕，建议画画图  
    Grade = []  
    Z = np.transpose(Z)  
    # 对每一个评价对象循环, len(Z[0])=m，循环m次  
    for i in range(0, len(Z[0])):  
        # 计算评价对象与最大值的距离  
        sum1 = 0  
        # 对每一个评价指标循环, len(Z)=n，循环n次  
        for j in range(0, len(Z)):  
            sum1 = sum1 + (Z[j][i] - Z_max[j]) ** 2  
        D_P = np.sqrt(sum1)  
  
        # 计算评价对象与最小值的距离  
        sum2 = 0  
        for j in range(0, len(Z)):  
            sum2 = sum2 + (Z[j][i] - Z_min[j]) ** 2  
        D_N = np.sqrt(sum2)  
  
        Grade.append(D_N / (D_P + D_N))  
  
    '''  
    # 方法二：直接减  
    Z_max = np.tile(Z_max, (len(Z[0]), 1))    
    D_P = np.sqrt(np.sum((Z_max - Z) ** 2, axis=1))  
    Z_min = np.tile(Z_min, (len(Z[0]), 1))    
    D_N = np.sqrt(np.sum((Z_min - Z) ** 2, axis=1))  
    Grade = D_N / (D_P + D_N)    '''    
    return Grade  
  
# 归一化函数：  
def norm(Grade):  
    sum = 0  
    for i in range(0, len(Grade)):  
        sum = sum + Grade[i]  
    for i in range(0, len(Grade)):  
        Grade[i] = Grade[i] / sum  
    return Grade  
  
# 注意：原本这个数组应该是每一列为不同的评价指标，但这里由于对列进行正向化不方便，所以先设置为每行代表一个指标  
#      到后面再对这个矩阵进行转置  
A = np.array([[28, 19, 3, 5, 3, 3, 8],  
              [1806, 3180, 3080, 2560, 3551, 3309, 3139],  
              [11.2, 7.98, 8.12, 5.29, 4.3, 5.68, 6.58],  
              [0.67, 0.71, 0.75, 0.76, 0.874, 0.92, 0.708],  
              [17413, 11770, 9081, 11566, 8295, 9143, 9556],  
              [0.823, 0.741, 0.804, 0.7947, 0.851, 0.754, 0.8676],  
              [0.112, 0.111, 0.1, 0.091, 0.071, 0.058, 0.067]])  
  
# 对矩阵进行正向化  
A[1] = positivization_2(A[1], 2500)  
A[3] = positivization_2(A[3], 0.74)  
A[6] = positivization_2(A[6], 0.105)  
  
# 转置为通常的正向化矩阵  
X = np.transpose(A)  
print(f'共有{len(X)}个评价对象，{len(X[0])}个评价指标')  
  
# 对正向化矩阵标准化  
Z = standardize(X)  
  
# 对评价对象打分  
Grade = grade(Z)  
  
# 对评分归一化  
Grade_norm = norm(Grade)  
print(f'未排序得分{Grade_norm}')  
  
# 对评分进行排序并返回索引  
Grade_norm_sorted = np.argsort(Grade_norm)  
print(f'排序得分{np.sort(Grade_norm)}')  
print(f'对应指标{Grade_norm_sorted}')
```


## 拓展：带权重的TOPSIS
对原本的归一化进行修改

**多了个$\omega$**
定义第 $i(i=1,2,...,n)$ 个评价对象与最大值的距离$D_i^+ = \sqrt{\sum_{j=1}^{m}\omega_j(Z_j^+-z_{ij})^2}$
定义第 $i(i=1,2,...,n)$ 个评价对象与最小值的距离$D_i^- = \sqrt{\sum_{j=1}^{m}\omega_j(Z_j^--z_{ij})^2}$

那么，可以计算出第 $i(i=1,2,...,n)$ 个评价对象未归一化的得分：$$S_i =\frac{D_i^-}{D_i^+ + D_i^-}$$
很明显$0 \leq S_i \leq 1$，且 $S_i$ 越大 $D_i$ 越小，即越接近最大值。
我们可以将得分归一化：$$\widetilde{S_i} = \frac{S_i}{\sum_{i=1}^n \widetilde{S_i} }$$

*如何确定 $\omega$ 大小?*
采用**层次分析法**或者**熵权法**

### 基于熵权法(EWM)对TOPSIS模型的修正
熵权法是一种客观赋权方法
**原理：** 指标的变异程度越小，所反映的信息量也越小，其对应的权值也越低（客观=数据本身就可以告诉权重）

>一种极端的例子：对于所有的样本而言，这个指标都是相同的数值，那么我们可认为这个指标的权值为0，即这个指标对于我们的评价起不到任何帮助

度量**信息量**：不确定性
![[微信截图_20230704170452.png]]

**信息熵：**
![[微信截图_20230704170558.png]]

随机变量的信息熵越大，则它的值（内容）能给你补充的信息量越大，而知道这个值前你已有的信息量越小。

#### 熵权法的计算步骤
##### 1. 判断输入的矩阵中是否存在负数，如果有则要重新标准化到非负区间（后面计算概率时需要保证每一个元素为非负数）
假设有 $n$ 个要评价的对象，$m$ 个评价指标（已经正向化）构成的正向化矩阵如下：  
$$X=\begin{equation}\begin{bmatrix}
x_{11} & x_{12}& \cdots & x_{1m} \\
x_{21} & x_{22}& \cdots & x_{2m} \\
\vdots & \vdots &\ddots & \vdots \\ 
x_{n1} & x_{n2} &\cdots & a_{nm} \end{bmatrix}\end{equation}$$
那么，对其标准化的矩阵记为 $Z$，$Z$ 中的每一个元素：$$z_{ij}=\frac{x_{ij}}{\sqrt{\sum_{i=1}^{n} x_{ij}^2}}$$
判断 $Z$ 矩阵中是否存在着负数，如果存在的话，需要对 $X$ 使用另一种标准化方法：
	对矩阵 $X$ 进行一次标准化得到 $$\widetilde{z_{ij}}=\frac{x_{ij}-min\{ x_{1j},x_{2j},...,x_{nj}\}}{max\{x_{1j},x_{2j},...,x_{nj}\}-min\{ x_{1j},x_{2j},...,x_{nj}\}}$$

##### 2. 计算第 j 项指标下第 i 个样本所占的比重，并将其看作相对熵计算中用到的概率
假设有 $n$ 个要评价的对象，$m$ 个评价指标（已经正向化）构成的标准化矩阵如下：
$$\widetilde{Z}=\begin{equation}\begin{bmatrix}
\widetilde{z_{11}} & \widetilde{z_{12}}& \cdots & \widetilde{z_{1m}} \\
\widetilde{z_{21}} & \widetilde{z_{22}}& \cdots & \widetilde{z_{2m}} \\
\vdots & \vdots &\ddots & \vdots \\ 
\widetilde{z_{n1}} & \widetilde{z_{n2}} &\cdots & \widetilde{z_{nm}} \end{bmatrix}\end{equation}$$
计算概率矩阵P，其中P中每一个元素 $p_{ij}$ 的计算公式如下：
$$p_{ij}=\frac{\widetilde{z_{ij}}}{\sum_{i=1}^n \widetilde{z_{ij}}}$$
容易验证： $\sum_{i=1}^n p_{ij}=1$，即保证了每一个指标对应的概率和为 $1$ 

##### 3. 计算每个指标的信息熵，并计算信息效用值，并归一化得到每个指标的熵权
对于第 j 个指标而言，其信息熵的计算公式为： $$e_j=-\frac{1}{\ln n}\sum_{i=1}^n p_{ij} \ln(p_{ij}) \ (j=1,2,...,,m)$$
**信息效用值（差异系数）：** $d_j = 1-e_j$，第 j 项指标的观测值差异越大，则差异系数$d_j$ 越大，所对应的信息越多，这项指标也越重要

**归一化得到熵权（权重系数）：** $W_j = \frac{d_j}{\sum_{j=1}^m d_j}\ (j=1,2,...,m)$ 

*Q: 为什么要除以$\ln n$ ?*
	当 $p(x_1)=p(x_2)=...=p(x_n)=\frac{1}{n}$ 时，$H(x)$ 取最大值，此时$H(x)=\ln n$，这里除以$\ln n$ 可以使得信息熵始终位于 $[0,\ 1]$ 区间上面

*Q: $e_j$ 越大，即第 j 个指标的信息熵越大，表明第 j 个指标的信息越多还是越少？*
	越少， 当$p_{1j}=p_{2j}=...=p_{nj}$ 时，$e_j=1$， 此时上面定义的信息熵达到最大。
	但是，因为$p_{ij} = \frac{\widetilde{z_{ij}}}{\sum_{i=1}^n \widetilde{z_{ij}}}$，所以$\widetilde{z_{1j}} = \widetilde{z_{2j}}=...=\widetilde{z_{nj}}$，即所有样本的这个指标值都相同，那么 $d_j$ 就会较小，对应的信息较少

##### 熵权法的优缺点
**优点**
1. 基于信息熵的思路，能够客观地反映不同指标之间的差异性和重要程度，计算结果较为准确；
2. 不受主观因素的影响，不存在专家打分或排斥某些指标的风险，计算结果更加客观可靠；
3. 能够处理多个因素之间的相互关系，考虑到不同指标之间的相关性和相互作用；
4. 具有稳定性和一致性，避免AHP等方法中可能存在的不一致性、不稳定性等问题；
5. 计算公式简单易懂，适用范围广泛。

**缺点**
1. 需要指标尽可能的符合[正态分布](https://www.zhihu.com/search?q=%E6%AD%A3%E6%80%81%E5%88%86%E5%B8%83&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%222969806473%22%7D)；
2. 指标之间不是完全独立的，熵权法不能考虑到指标之间的相关性；
3. 熵权法受数据误差的影响比较大，在实际运用中需要注意数据的准确性

熵权法的深入理解：[如何用熵权法计算权重？ - 疯狂绅士的回答 - 知乎 ](https://www.zhihu.com/question/357680646/answer/1748591262)

### 代码实现结合熵权法
注：
1. 前面的正向化和标准化相差其实不大（除了要注意是否有指标是负数以外）
2. 主要是在打分部分的权重如何计算以及如何赋值
3. 代码可能有误，后面再修正
4. 注意当`P[i][j] =0`时，`ln(P[i][j])`此时为nan，需要修正
5. *Q: 当什么时候赋权法的出来的答案与不赋权答案近似？*
```python
import numpy as np  
  
# 定义正向化函数：  
# 极小型->极大型  
def positivization_1(X):  
    M = np.max(X)  
    # len(X)与X.shape(0)效果一样（二维数组情况下）  
    for i in range(0, len(X)):  
        X[i] = M-X[i]  
    return X  
  
# 中间型->极大型  
def positivization_2(X, best):  
    # 一个向量与一个常数做加减乘除，相当于对向量中的每一个数进行加减乘除  
    M = np.max(np.abs(X - best))  
    for i in range(0, len(X)):  
        X[i] = 1 - np.abs(X[i] - best)/M  
    return X  
  
# 区间型->极大型  
def positivization_3(X, a, b):  
    M = max(a - np.min(X), np.max(X) - b)  
    for i in range(0, len(X)):  
        if X[i] <= a:  
            X[i] = 1 - (a-X[i])/M  
        elif X[i] >= b:  
            X[i] = 1 - (X[i]-b)/M  
        else:  
            X[i] = 1  
    return X  
  
#定义标准化函数：  
def standardize(X):  
  
    # axis=0, 压缩行，即将每一列的元素相加，将矩阵压缩成一行  
    SUM_square = np.sum(X ** 2, axis=0)  
    # 将矩阵扩充为与原本大小相同  
    SUM = np.tile(SUM_square, (len(X[0]), 1))  
    Z = X / np.sqrt(SUM_square)  
    return Z  
  
# 计算概率矩阵P  
def probability_matrix(Z):  
    # 获得 1xm 的矩阵，每个元素为对应列所有元素之和  
    SUM = np.sum(Z, axis=0)  
    SUM = np.tile(SUM, (len(Z), 1))  
    P = Z / SUM  
    return P  
  
# 计算信息熵  
def entorpy_calculate(P):  
    E = np.zeros(len(P[0]))  
  
    # 将 P 中小于等于 0 的元素替换为 0.0001    for i in range(0, len(P)):  
        for j in range(0, len(P[0])):  
            if P[i][j] <= 0.0001:  
                P[i][j] = 0.001  
  
    E = (-1.0 / np.log(len(P[0]))) * np.sum(P * np.log(P), axis=0)  
    D = np.ones(len(P[0])) - E  
    W = D / np.sum(D)  
    print(f'熵权法确定的权重为{W}')  
    return W  
  
# 定义打分函数：  
def grade(Z, W):  
    # 获得每一列的最大值/最小值，返回的是一个1xn的行向量（矩阵为mxn)  
    Z_max = np.max(Z, axis=0)  
    Z_min = np.min(Z, axis=0)  
  
    SUM1 = np.zeros(len(P))  
    SUM2 = np.zeros(len(P))  
    D_P = np.zeros(len(P))  
    D_N = np.zeros(len(P))  
  
    for i in range(0, len(P)):  
        for j in range(0, len(P[0])):  
            SUM1[i] += W[j] * ((Z_max[j] - Z[i][j]) ** 2)  
            SUM2[i] += W[j] * ((Z_min[j] - Z[i][j]) ** 2)  
        D_P[i] = np.sqrt(SUM1[i])  
        D_N[i] = np.sqrt(SUM2[i])  
  
    Grade = D_N / (D_P + D_N)  
  
    return Grade  
  
# 归一化函数：  
def norm(Grade):  
    sum = 0  
    for i in range(0, len(Grade)):  
        sum = sum + Grade[i]  
    for i in range(0, len(Grade)):  
        Grade[i] = Grade[i] / sum  
    return Grade  
  
# 注意：原本这个数组应该是每一列为不同的评价指标，但这里由于对列进行正向化不方便，所以先设置为每行代表一个指标  
#      到后面再对这个矩阵进行转置  
A = np.array([[28, 19, 3, 5, 3, 3, 8],  
              [1806, 3180, 3080, 2560, 3551, 3309, 3139],  
              [11.2, 7.98, 8.12, 5.29, 4.3, 5.68, 6.58],  
              [0.67, 0.71, 0.75, 0.76, 0.874, 0.92, 0.708],  
              [17413, 11770, 9081, 11566, 8295, 9143, 9556],  
              [0.823, 0.741, 0.804, 0.7947, 0.851, 0.754, 0.8676],  
              [0.112, 0.111, 0.1, 0.091, 0.071, 0.058, 0.067]])  
  
# 对矩阵进行正向化  
A[1] = positivization_2(A[1], 2500)  
A[3] = positivization_2(A[3], 0.74)  
A[6] = positivization_2(A[6], 0.105)  
  
# 转置为通常的正向化矩阵  
X = np.transpose(A)  
print(f'共有{len(X)}个评价对象，{len(X[0])}个评价指标')  
  
# 对正向化矩阵标准化  
Z = standardize(X)  
  
# 获得对应权重  
P = probability_matrix(Z)  
W = entorpy_calculate(P)  
  
# 对评价对象打分  
Grade = grade(Z, W)  
  
# 对评分归一化  
Grade_norm = norm(Grade)  
print(f'未排序得分{Grade_norm}')  
  
# 对评分进行排序并返回索引  
Grade_norm_sorted = np.argsort(Grade_norm)  
print(f'排序得分{np.sort(Grade_norm)}')  
print(f'对应指标{Grade_norm_sorted}')
```