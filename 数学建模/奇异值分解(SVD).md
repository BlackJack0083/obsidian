>[矩阵降维](https://www.zhihu.com/search?q=%E7%9F%A9%E9%98%B5%E9%99%8D%E7%BB%B4&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%2274245918%22%7D)与矩阵分解是两个不同的概念。
>矩阵降维指就是把数据从高维空间*投影*到一个低维空间，这个过程可以通过线性或者非线性的映射来完成。目的是挖掘出高维数据中富含原始信息的低维嵌入表示。降维显然是有代价的，它造成了*原始信息的损失*。所以[降维算法](https://www.zhihu.com/search?q=%E9%99%8D%E7%BB%B4%E7%AE%97%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%2274245918%22%7D)的重点和难点在于如何在对原始数据进行数据降维的过程中还能尽可能地*保持高维数据的几何结构信息或本征的有区别性的信息*，并在此前提下找到高维数据的最优低维表示。
>对于传统的降维算法来说，它们通常的考虑角度都是找到最大可能地保持某种信息的投影方向或者低维空间。
>而矩阵分解是将矩阵拆解为数个矩阵的乘积，并**没有改变矩阵的维数**，但通过矩阵分解往往可以从复杂的数据中提取出相对重要的特征信息，如在特征向量为基的各个方向上的投影，然后通过**保留较大的投影，删除较小的投影**，便可实现降维的目的；例如特征分解

奇异值分解（Singular Value Decomposition）是线性代数中一种重要的矩阵分解，其在图形学、统计学、推荐系统、信号处理等领域有重要应用。

对角化定理在许多应用中均很重要, 然而不是所有矩阵都有分解式 $A = P DP ^{-1}$ 且 $D$ 是对角的. 但分解 $A = QDP ^{-1}$, ($Q, P$ 都是单位正交阵, $D$ 是对角阵) 对任意 $m × n$ 矩阵 $A$ 都是可行的, 甚至*不需要 $A$ 是方阵*

**奇异值分解的思想**就是*模仿对称矩阵对角化*的过程处理长方形矩阵: 一个对称矩阵 $A$  的特征值的绝对值表示 $A$ 拉长或压缩一个向量 (特征向量) 的程度. 如果 $Ax = λx$, 且  $||x|| = 1$, 那么  $$||Ax|| = ||λx|| = |λ|||x|| = |λ|$$
若 $λ_1$ 是绝对值最大的特征值, 对应的单位特征向量 $v_1$ 确定了 $A$ 的*最大拉伸方向*.
### 基础知识回顾
![[微信截图_20230829104429.png]]![[微信截图_20230829104457.png]]![[微信截图_20230829104515.png]]![[微信截图_20230829104534.png]]![[微信截图_20230829104546.png]]![[微信截图_20230829104602.png]]

# 奇异值分解
## 引理
(1) $AB$ 和 $BA$ 非零的*特征值完全相同* (非零特征值的重数也相同)
(2) 实对称矩阵*特征值一定为实数*，且一定可以相似对角化，特征向量构成的矩阵可通过施密特正交化(Schmidt orthogonalization)变为*正交矩阵*
(3) $AA^T$ 一定是半正定矩阵，因此其*特征值不可能为负数*

## 定理
设 $A$ 是秩为 $r$ 的 $m×n$ 矩阵, 那么存在一个 $m×n$ 矩阵 $Σ = \begin{bmatrix} &D_r &O_{r,n-r} \\ &O_{m -r,r} &O_{m-r,n-r}   \end{bmatrix}$，其中对角阵 $D_r$的对角线元素是 $A$ 的前 $r$ 个奇异值, $σ_1 ⩾ σ_2 ⩾ · · · ⩾ σ_r > 0$ 且 $\sigma_1^2,\sigma_2^2,...,\sigma_r^2$ 为矩阵$A^TA$对应的正特征值, 并且存在一个 $m$ 阶正交矩阵 $U$ 和一个 $n$ 阶正交矩阵 $V$ 使得  
$$A_{m \times n} = U_{m \times m}Σ_{m \times n}V ^T_{n \times n}$$
称为 $A$ 的奇异值分解.
![[微信截图_20230829112102.png]]

### 关于$U、V、\Sigma$的计算
![[微信截图_20230829112918.png]]![[微信截图_20230829112928.png]]![[微信截图_20230829112938.png]]

## SVD的作用：降维
![[微信截图_20230829113807.png]]

舍去奇异值较低的部分，避免稀疏矩阵且保留较多信息。

### 实际应用1：推荐算法
来自教材：
>大众点评中有个原始打分表，行表示用户，列表示菜品，用户对菜打1~5分，分数越高表示评价越高，如果没有品尝过则默认为0分，有如下评分表（见附件），希望估计未评分项目的评分，从而为推荐菜品提供帮助

**问题： 如何知道用户的未品尝菜品的得分？**
*协同过滤：* 先通过其他所有用户的评价记录，来衡量菜品与用户评价过的其他菜品的相似程度，利用该用户对于其他菜品的已评分数和菜品间的相似程度，估计出该用户会对这个未评分菜品打出多少分

**关键：**
	1. 衡量菜品之间的相似性
	2. 评分估计

#### 非压缩数据的模型1
##### 相似系数计算
采用相关系数。设$x_1,x_2,...,x_{11}$表示菜品，用$i=1,2...,18$表示用户。记用户$i$关于指标变量$x_j(j=1,2...,11)$的评分值为$a_{ij}$，构造数据矩阵$A=(a_{ij})_{18\times 11}$ 

计算$x_i, x_j$(两个菜品)之间的相关系数:$$r_{ij}=\frac{\sum_{k=1}^{18}(a_{ki}-\mu_i)(a_{kj}-\mu_j)}{\sqrt{\sum_{k=1}^{18}(a_{ki}-\mu_i)^2}\sqrt{\sum_{k=1}^{18}(a_{kj}-\mu_j)^2}}$$式中，$\mu_j = \frac{1}{18}\sum_{k=1}^{18}a_{kj}$，当$i=j$时，$r_{ij}=1$

相关系数的取值范围为-1~1，可对其归一化处理，即通过$0.5+0.5r_{ij}$将相关系数归一化到0~1的范围。此时，值越接近 $1$ 说明两个变量的相似度越高，即两菜品相似程度越高。

>这个地方一开始的形式比较古怪，但是转换成概率论与数理统计当中的相关系数理解可能会好一点。上式可化简为$r_{ij}=\frac{cov(i,j)}{s_i \cdot s_j}$，就跟当时学的相关系数形式差不多了。只是当时学的是概率论下的相关系数计算，此处变为了统计下的计算。那么这里的解释的话就可以当成第 $j$ 个菜品与第 $i$ 个菜品的相似程度，只是由于要以每种菜品都作为一次主变量，所以会得到一个$11 \times 11$ 的方阵

##### 评分估计
利用该顾客已经评过分的菜品分值，来评估某个未评分的菜品分值。

令要估计的菜品为$x_k$，该顾客已经评过分的菜品为$x_j(j=j_1,j_2,...,j_{n_k})$，评过的分数分别为$s_j(j=j_1,j_2,...,j_{n_k})$，$x_k$与$x_j$的相似程度为$\tilde r_{kj}(j=j_1, j_2,...,j_{n_k})$，由此，利用相似度加权的方式来估计$x_k$的评分$$\hat s_k = \frac{\sum_{i=1}^{n_k}\tilde r_{kj_i}s_{j_i}}{\sum_{i=1}^{n_k}\tilde r_{kj_i}}$$

##### 代码实现
```python
import numpy as np  
import pandas as pd  
  
a = np.loadtxt('data3_6_1.txt')  
b = 0.5 * np.corrcoef(a.T) + 0.5  # 获得相关系数矩阵，并求归一化的相似度  
c= pd.DataFrame(b)  
c.to_excel('data3_6_2.xlsx', index=False)  
  
print('请输入人员编号1-18')  
user = int(input())  
n = a.shape[1]  #变量的个数  
no = np.where(a[user-1, :]==0)[0] #未评分编号  
yb = set(range(n)) - set(no)  #已评分编号  
yb = list(yb)  
ys = a[user-1, yb]  #已评分分数  
sc = np.zeros(len(no))  #初始化  
for i in range(len(no)):  
    sim = b[no[i], yb]  
    sc[i] = ys @ sim / sum(sim)  
print('未评分项的编号为：', no+1)  
print('未评分项的分数为：', np.round(sc, 4))
```

#### 基于奇异值分解压缩数据的模型2
##### 稀疏矩阵的降维
在原始数据矩阵中，记录了18位顾客对11道菜品的全部打分情况，但由于每个人不太可能吃遍美食平台上的每一道菜品，因此这个矩阵一定是稀疏矩阵，拥有大量的0元素项。这样，虽然一方面从表面现象看矩阵的维数很高；但是从另一方面看，某个顾客同时对两道菜品打分的情况不一定普遍。
可以基于奇异值分解对原始数据进行行压缩。对$A$进行奇异值分解，得到：$$A = U_{18\times 18}\Sigma_{18 \times 11} V^T_{11\times 11} $$式中：$Σ = \begin{bmatrix} D_{11} \\ O_{7,11} \end{bmatrix}$，$D_{11} = diag(\sigma_1,..,\sigma_{11})$，这里的$\sigma_i$为矩阵$A$的奇异值，可以验证：$$\frac{\sum_{i=1}^6 \sigma_i^2}{\sum_{i=1}^{11} \sigma_i^2} \ge0.9$$因此只需要6个特征值，就可以使其主成分贡献率达到$90\%$。于是，我们就可以通过行压缩的方式，将原始分数矩阵的行有18行压缩到6行，避免稀疏矩阵的情况。压缩后的数据矩阵$$B_{6 \times 11}=diag(\sigma_1,\sigma_2,...,\sigma_6)U^T[:6,:]A$$式中：$U^T[:6,:]$为由$U^T$的前6行所组成的6行18列矩阵

##### 计算相似性
采用余弦相似度计算
	对于两个指定向量$v_1，v_2$，二者的余弦相似度就是用二者夹角$\theta$的余弦值$cos \theta$来表示，即$$cos\theta = \frac{v_1 \cdot v_2}{|v_1||v_2|}$$余弦值的取值范围为-1~1，可以通过归一化，计算$0.5+0.5\frac{v_1 \cdot v_2}{|v_1||v_2|}$将余弦相似度化到0~1的范围内。此时，值越接近1代表相似度越高。记得到的菜品之间的相似性矩阵为$\tilde R = (\tilde r_{ij})_{11 \times 11}$

##### 评分估计
同上

##### 代码
```python
#程序文件ex3_6_2.py  
import numpy as np  
import pandas as pd  
  
a = np.loadtxt('data3_6_1.txt')  
u, sigma, vt = np.linalg.svd(a)  
print(sigma)  
cs = np.cumsum(sigma**2)    
rate = cs / cs[-1]  #计算信息累积贡献率  
ind = np.where(rate>=0.9)[0][0]+1  
#ind为奇异值的个数，满足信息提出率达到90%  
b = np.diag(sigma[:ind]) @ u.T[:ind, :] @ a  #得到降维数据  
  
c = np.linalg.norm(b, axis=0, keepdims=True)  #逐列求范数  
d = 0.5 * b.T @ b / (c.T @ c) + 0.5  #求相似度  
#d = 0.5 * np.corrcoef(b.T) + 0.5  
dd = pd.DataFrame(d)  
dd.to_excel('data3_6_3.xlsx', index=False)  
  
print('请输入人员编号1-18')  
user = int(input())  
n = a.shape[1]  #变量的个数  
no = np.where(a[user-1, :]==0)[0] #未评分编号  
yb = set(range(n)) - set(no)  #已评分编号  
yb = list(yb)  
ys = a[user-1, yb]  #已评分分数  
sc = np.zeros(len(no))  #初始化  
for i in range(len(no)):  
    sim = d[no[i], yb]  
    sc[i] = ys @ sim / sum(sim)  
print('未评分项的编号为：', no+1)  
print('未评分项的分数为：', np.round(sc,4))
```

### 图片压缩
奇异值分解在图像处理中有着重要的作用。假设一幅图像有$m \times n$个像素，如果将$mn$个数据一起传输，往往会显得数据量太大。因此，我们希望能够改为*传送另外一些比较少的数据*，并且能够在接收端利用这些传输的数据*重构原图像*。

用矩阵$A$表示要传送的原$mn$个像素。假设对矩阵进行奇异值分解，得到$A = USV^T$，其中奇异值按照从大到小的顺序。如果从中选择$k$个大奇异值以及与这些奇异值对应的左和右奇异向量逼近原图像，便可以使用$m \times k + k +k\times n = k(m+n+1)$个数值代替原来的$m \times n$个图像数据。压缩比率为（也有直接算百分比的算法，此处参考教材）$$\rho = (1-\frac{k(m+n+1)}{mn})\times 100\%$$
因此，在传送图像时就无需传送$mn$个原始数据，只需要$k(m+n+1)$个有关奇异值和奇异向量的数据即可。在接收端，在收到奇异值$\sigma_1, \sigma_2,...,\sigma_k$以及左奇异向量$u_1, u_2,...,u_k$和右奇异向量$v_1,v_2,...,v_k$后，即可通过以下截尾的奇异值分解公式重构出原图像：$$\hat A = \sum_{i=1}^k\sigma_i u_i v_i^T$$
*注意：* 若$k$偏小，记压缩比例$\rho$偏大，则重构的图像质量可能不能令人满意。反之过大的$k$值又会导致压缩比例国小，从而降低图像压缩和传送的效率。因此，需要根据不同种类的图像，选择合适的压缩比，以兼顾图像传输效率和重构质量。

```python
#程序文件ex3_7.py  
import numpy as np  
from numpy import linalg as LA  
from PIL import Image  
import pylab as plt  #加载Matplotlib的Pylab接口  
plt.rc('font', size=13)  
plt.rc('font', family='SimHei')  
a = Image.open("Lena.bmp")  #返回一个PIL图像对象  
if a.mode != 'L':  
    a = a.convert("L")  #转换为灰度图像  
b = np.array(a).astype(float)  #把图像对象转换为数组  
[p, d, q] = LA.svd(b)  
m,n=b.shape  
R = LA.matrix_rank(b)  #图像矩阵的秩  
plt.figure(0)  
plt.plot(np.arange(1,len(d)+1),d,'k.')  
plt.ylabel('奇异值');  
plt.title('图像矩阵的奇异值')  
CR=[]  
for K in range(1,int(R/4),10):  
    plt.figure(K)  
    plt.subplot(121)  
    plt.title('原图')  
    plt.imshow(b, cmap='gray')  
    I = p[:,:K+1] @ (np.diag(d[:K+1])) @ (q[:K+1,:])  
    plt.subplot(122)  
    plt.title('图像矩阵的秩='+str(K))  
    plt.imshow(I, cmap='gray')  
    src=m*n; compress=K*(m+n+1)  
    ratio=(1-compress/src)*100  #计算压缩比率  
    CR.append(ratio)  
    print("Rank=%d:K=%d个：ratio=%5.2f"%(R,K,ratio))  
plt.figure(); plt.plot(range(1,int(R/4),10),CR,'ob-');  
plt.title("奇异值个数与压缩比率的关系"); plt.xlabel("奇异值个数")  
plt.ylabel("压缩比率"); plt.show()
```

### SVD的评价与应用
#### 评价
1.优点: 简化数据，去除噪声点，对数据降维 (减少秩)
2.缺点:数据的转换可能难以理解
3.适用于数据类型:数值型。
通过SVD对数据的处理，我们可以对原始数据进行精简，这样做实际上是去除了噪声和几余信息，以此达到了优化数据的目的。

#### SVD的另外两个重要应用
- *潜在语义索引*: 最早的SVD应用之一就是信息检索我们称利用SVD的方法为潜在语义检索 (LSI) 或隐形语义分析 (LSA)有兴趣可以去阅读吴军老师的《数学之美》
- *推荐系统*: SVD的另一个应用就是推荐系统，较为先进的推荐系统先利用SVD从数据中构建一个主题空间，然后再在该空间下计算相似度，以此提高推荐的效果。