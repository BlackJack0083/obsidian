一开始看感觉没什么问题，结果仔细想一直没想清楚一开始是怎么构建多边形的。。。在这里详细进行介绍
相关链接：[技术分享：Delaunay三角剖分算法介绍 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/459884570)
[从自然到空间-认识Voronoi（泰森多边形） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/48252861)
# 基本介绍
**沃罗诺伊图**（Voronoi Diagram，也称作Dirichlet tessellation，狄利克雷镶嵌）是由乌克兰数学家[格奥尔吉·沃罗诺伊](https://zh.wikipedia.org/wiki/%E6%A0%BC%E5%A5%A5%E5%B0%94%E5%90%89%C2%B7%E6%B2%83%E7%BD%97%E8%AF%BA%E4%BC%8A)建立的空间分割算法。灵感来源于[笛卡尔](https://zh.wikipedia.org/wiki/%E7%AC%9B%E5%8D%A1%E5%B0%94 "笛卡尔")用凸域分割空间的思想。在几何、晶体学、建筑学、地理学、气象学、信息系统等许多领域有广泛的应用。

沃洛诺伊图的单元被称为**泰森多边形**。

Voronoi图在网格化中具有灵活性和可扩展性，可以用于生成各种类型的网格结构，可以用于从离散的点集合中重建连续的曲面或表面。通过计算点集的Voronoi图，可以生成一组多边形或三角形网格，这些网格可以近似原始点集的表面形状。

![image.png|320](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/20240323230826.png)

# 性质
1. 离散性：Voronoi图将空间划分为离散的区域，每个区域都与一个生成点相关联。
2. 连续性：Voronoi边界是连续的线段，由相邻的生成点之间的距离确定。
3. 局部性：每个点的Voronoi区域仅由其最近的邻近点决定，与其他点的位置无关。

# 构建方法
1. 输入：给定一组点作为输入，称为生成点或种子点。假设有限的正方形平面区域中有若干个特定点（橙色点）：
![image.png|425](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/20240323232148.png)
2. 连线：通过连接每对相邻的生成点，形成一系列的边界线段，这些线段称为Voronoi边。基于特定点集进行德劳内三角剖分(Delaunay triangulation)。(德劳内三角剖分是Voronoi图的对偶图，其介绍可以看看[Delaunay triangulation](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Delaunay_triangulation))。这一过程是绘制沃罗诺伊图的核心，也是整个绘图过程计算量的关键。
![image.png|425](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/20240323232228.png)
3. 每一个特定点会与其它若干个特定点组成多个[德劳内三角形](https://www.zhihu.com/search?q=%E5%BE%B7%E5%8A%B3%E5%86%85%E4%B8%89%E8%A7%92%E5%BD%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A96311519%7D)，而我们需要的是这些三角形的外心（蓝色点）。根据外心的性质，其到三角形的三点的距离相等，也是我们需要的的顶点。
![image.png|400](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/20240323232441.png)
4.  因为是[有限正方形](https://www.zhihu.com/search?q=%E6%9C%89%E9%99%90%E6%AD%A3%E6%96%B9%E5%BD%A2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A96311519%7D)区域，所以我们用边界直接截取得到[边界交点](https://www.zhihu.com/search?q=%E8%BE%B9%E7%95%8C%E4%BA%A4%E7%82%B9&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A96311519%7D)。同时我们将边界交点和外心（黄色点）分别关联到对应的特定点。
![image.png|400](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/20240323232513.png)
5. 某一特定点的关联点顺时针或者逆时针相连就得到沃罗诺伊小元胞，而将所有特定的关联点分别相连就得到沃罗诺伊图
![image.png|400](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/20240323232540.png)
6. 沃罗诺伊图。![image.png|425](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/20240323232554.png)
7. 根据各个元胞的面积抹上颜色
![image.png|400](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/20240323232615.png)

### 附：Delaunay三角剖分
![397b94ec96c13e1016a3163919dda7d.png](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/397b94ec96c13e1016a3163919dda7d.png)

Delaunay三角形化方法，是将平面上一组已给定的点连成三角形的方法，这些连接而成的三角形有以下特点:
(1) 所形成的三角形**互不重叠**;
(2) 所形成的三角形可以覆盖整个平面;
(3) 每个点均不位于不包含该点的三角形的**外接圆**内*(即:在某个三角形的外接圆内，只包含在外接圆上的三个节点，不包含其他节点)

Delaunay三角形化的优点
(1)Delaunay三角形化方法所连接成的各三角形中的**最小角**对给的的这一组点是各种连接方式的**最大者***(最小角的最大化);
(2)生成的三角形边长的均匀性最好，最接近**正三角形**

![1bcd23c85e47d8e2fdb1015beeb7a9e.png](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/1bcd23c85e47d8e2fdb1015beeb7a9e.png)
![396be8e7a3f759150ab58c95359bbb4.png](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/396be8e7a3f759150ab58c95359bbb4.png)

使用Delaunay三角形化生成网格所存在的问题
1. 边界匹配问题，即求解区域的边界不是Delaunay三角形的边
2. 如何向求解区域内设点(如上页ppt的Q点)?

![fb3d0aceb40c097ce5980a3ad7e8fad.png](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/fb3d0aceb40c097ce5980a3ad7e8fad.png)
![f5d625562665b3c3e796aea9ee7a12f.png](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/f5d625562665b3c3e796aea9ee7a12f.png)

2. 向求解区域内设点问题
向初始化的Delaunay三角形内加点的过程是生成非结构网格的关键步骤，需要满足以下的三个基本要求:
(1)过程能自动的进行，即所加入的点的位置的选择有一定的规则可依据，因而可由计算机按这种原则来确定，无需人工干预;
(2)这种内点的生成能将边界上已经设定的网格点疏密程度光滑的传递到到求解区域内部;
(3)能方便的控制区域内点(或某一条直线)附近网格的疏密

3. 两个几何参数
为满足上述三个基本要求，需要引入两个参数:长度标尺三角形外接圆无量纲半径
![41004880ea0c8b2a9a5a0909af884f6.png](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/41004880ea0c8b2a9a5a0909af884f6.png)
两个参数的意义
(1)长度标尺
长度尺度的大小代表了边界上网格疏密的程度，网格点稠密的地方，长度尺度小，而稀疏处长度尺度大;
(2)无量纲半径
判断三角形偏离正三角形的严重程度，$R(k)$越大偏离越严重，可以用来比较两个三角形偏离正三角形的相对程度，为**计算机自动生成内点提供依据**，即首先应该往$R(k)$较大的三角形中加入内点，以改善网格的质量

![27031cc24ecd0a23cac8cf0b11f1ed8.png](https://cdn.jsdelivr.net/gh/BlackJack0083/obsidian@main/img/27031cc24ecd0a23cac8cf0b11f1ed8.png)
