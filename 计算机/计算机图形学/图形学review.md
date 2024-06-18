# chap1
1. 什么叫计算机图形学？计算机图形学的研究对象是？
计算机图形学是研究通过计算机将数据转换为图形，并在专门的显示设备上显示的原理、方法和技术的学科

2. 图形与图像的区别（像素、灰度）
狭义概念中，通常把位图看作图像，把矢量图看作图形
位图通常用点阵法表示，用具有灰度或颜色信息的点阵表示图形
矢量图通常使用参数法来表示，以计算机中所记录图形的形状参数与属性参数来表示图形

3. 图形学的相关学科
计算几何、图像处理、计算机视觉
# chap 2 图形系统
1. 图形系统组成
计算机图形硬件、计算机图形软件

2. 名词概念：刷新频率、帧缓存、分辨率、图形软件标准
- 刷新频率 Refresh rate
	在屏幕上重复画图的频率 
- 像素Pixel
	每个屏幕上的点
- 帧缓存frame buffer
	显示屏幕图像所需要的存储空间
	帧frame—整个屏幕范围
- 分辨率resolution
	屏幕图像的密度。水平线上的点数 \* 垂直线上的点数
- 图形软件标准：系统各界面之间进行数据传递和通信的接口标准
	- 使用原因：降低开发应用程序成本，使程序具有可移植性

3. 英文名词的全称、含义：CRT、CPU、GPU、DAC、OpenGL
- CRT: 阴极射线管显示器
![image.png|625](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609094941.png)

通过调节电子枪发出的电子束中所含**电子的多少**，可以控制击中的相应荧光点的亮度，因此以不同的强度击中荧光点，就能够在像素点上生成极其丰富的颜色。如图是一个具有**24位面**的帧缓冲存储器，红、绿、蓝各8个位面，其值经数模转换控制红、绿、蓝电子枪的强度，每支电子枪的强度有**256(8位)个等级**，则能显示$256*256=16兆$种颜色，16兆种颜色也称作 **(24位)真彩色**。![image.png|650](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609095307.png)
荫罩式CRT:三角形排列荧光点
- CPU: 中央处理器
- GPU：图形处理单元，主要任务是对系统输入的视频信息进行构建和渲染，集成各图形函数
- RAMDAC：数字/模拟转换器：将二进制的数字转换成为与显示器相适应的模拟信号
- OpenGL：跨平台应用的API，相比DirectX的突出优点是跨平台性，还是图形库和图形软件标准

4. 颜色模型
- RGB(red, green, blue)
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609094715.png)
- CMY:RGB反色(注意black是(1,1,1,)，white是(0,0,0)), [C M Y]=[1 1 1]-[R G B]
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609094739.png)
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610100639.png)

5. 图形输入设备？图形输出设备？
- 图形输入设备：键盘、鼠标、触摸屏、扫描仪等
- 图形输出设备：打印机、绘图仪、激光照排设备

显示颜色64k(16 bit),分辨率为1024\*1024的显示器，至少需要的帧缓存容量为( D)
A 3MB
B 512KB
C 1MB
D 2MB
64kb=2^16b, 每个像素需要16个bit位（二进制位）来表示颜色，每8个bit位为一个字节，所以每个像素的颜色表示的容量为2字节，又因为分辨率单位为像素，

所以  像素个数\*每个像素颜色容量=帧缓存容量 => $2^{10}*2^{10}*2=2MB$

# chap3 基本图元生成算法
## 扫描转换
从图形定义的物理空间到显示处理的图像空间的转换，称为扫描转换或称为光栅化。
### 画线
#### DDA
优点：直观简单；
缺点：有浮点运算、每一步均需要圆整(四舍五入)、长线段会有积累误差
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609120307.png)
- 当|斜率|<=1的时候，x轴方向为步长方向
- 当|斜率|>1的时候，y轴方向为步长方向
#### Bresenham
避免浮点运算、没有圆整运算、消除积累误差
$$d = 2\Delta y - \Delta x$$
$$\begin{cases}d < 0, &d += 2 \Delta y, &(x_i + 1, y_i)\\ d \ge 0, &d +=2 (\Delta y - \Delta x), &(x_i+1, y_i + 1)\end{cases}$$
### 中点画圆
圆的方程$X^2+Y^2=R^2$。如果直接根据圆的方程画图，有什么问题？需要开平方根，计算量较大，且会产生像素间距不一致问题
试列举1种圆的生成算法，并说出它的优点。中点画圆法，去掉浮点运算，提高运算效率

中点画圆法。试找出一种办法使得候选像素点的数量最少，即用算法生成一个点后，圆上的其它点也自动生成? 并画图说明。圆的对称性

引入NE和E的中点M$(x_{i+1}, y_{i-0.5})$
![image.png|525](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609124015.png)

判别式$$d=F(M)=F(x_i+1,y_i-0.5)=(x_i+1)^2+(y_i-0.5)^2-R^2$$
$$\begin{cases}d<0, &(x_i+1,y_i)\\d \ge0, &(x_i + 1, y_i - 1)\end{cases}$$
化简后：$$h_i = d_i - 0.25,h_0 = d_0-0.25 = 1-R$$
$$\begin{cases}h_i < 0, &(x_i+1,y_i), &h_{i+1}=h_i+2x_i +3\\h_i > 0, &(x_i + 1, y_i - 1), &h_{i+1}=h_i+2x_i-2y_i+5 \end{cases}$$
### 反走样
由光栅算法生成的图元有锯齿效应，像这种由低频率采样引起的变形称为走样，可通过反走样技术解决

1. 增加采样频率，用更高的分辨率表示物体(少用，帧缓存不足)
2. 过采样：把屏幕看成由比实际更细的网格所覆盖，从而增加取样频率。实质是在高分辨率网格下采样，在低分辨率下显示

如图，将经过直线的像素分为9个子像素，只有2个子像素穿过该直线。已知直线反走样之前该像素的亮度等级为A,则反走样之后亮度等级变为多少？2/9
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609125445.png)
### 多边形扫描转换
多边形扫描转换算法中，一般来说，扫描线上区间点的前面有数个交点，如果交点的个数为**奇数对**时，则填**多边形色**; **偶数对**时,则填**背景色**。
#### 步骤
1. 求交Intersecting ：求扫描线与多边形各边的交点; 
2. 排序Sorting：将求得的交点按递增顺序进行排序; 
3. 交点配对Pair：确定相交区间; 
	- 特殊情况处理，当交点相连边的Y值在**局部形成极值**，交点计数**2个**；交点相连边的Y值单调**递增和递减**时，交点计数**1个**。
4. 区间填色Filling：区间内：成多边形色, 区间外：背景色。 
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609131631.png)

### 种子填充算法
种子填充算法搜索时如果相邻点在区域内，则该相邻点就成为**新的种子点**,以此继续**递归**搜索。
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609131209.png)
八连通加多四个方向即可

# chap 4 图形几何变换
- 二维几何变换
	齐次坐标：使得所有几何变换表示成矩阵连乘形式
	基本变换
	复合变换-三步曲
- 三维几何变换
	基本变换
	复合变换-三步曲
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609133822.png)

通常图形在方向、尺寸方面的变化是通过**图形的几何变换**来完成的  
使用 **齐次坐标**, 可将所有变换矩阵可转换成矩阵连乘形式。
ＸＸ矩阵描述的是**缩放、比例**变换。
3D旋转时， 什么方向转为正旋转角方向 ？  

矩阵乘法不满足交换律
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609154406.png)
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609155230.png)
# chap 5 三维观察
- 三维观察流程
- 平行投影
- 透视投影
### 观察流程
![image.png|600](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609160454.png)

- OpenGL**世界坐标**原点取决于 裁剪窗口设置函数gluOrtho2D和视区设置函数glViewport的设定
- OpenGL**屏幕坐标**系位于 **屏幕左上角**
- 观察函数 `gluLookAt(GLdouble eyex, GLdouble eyey,GLdouble eyez,GLdouble atx,GLdouble aty,GLdouble atz,GLdouble upx,GLdouble upy,GLdouble upz)`中的9个参数中456参数为目标点的坐标信息
### 平行投影与透视投影
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609161130.png)

- 平行投影parallel projection
	平行光源
	物体的投影线**相互平行**
	物体的**大小比例不变**，精确反映物体的实际尺寸

- 透视投影perspective projection
	点光源
	物体的投影线**汇聚成一点**：投影中心
	离**投影面近的物体生成的图像大**，真实感强
	透视投影的观察体是四棱台
	透视投影函数`void gluPerspective(GLdouble fov,GLdouble aspect, GLdouble near,GLdouble far)`，当第一个参数变大时，其他不变，物体相对变小(广角)

灭点：任何一束不平行于投影平面的平行线的透视变换将汇聚为一点。和投影中心不一样
![image.png|475](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609161206.png)一点透视、两点透视、三点透视
#### 投影分类
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609161259.png)
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609161354.png)

1、一般来说，**相机向上的方向**为**观察坐标系下的y方向**
2、目标点与视点的**连线方向**为**观察坐标系下的z方向**
3、从世界坐标系到观察坐标系的变换一般经过哪些变换？
	1) 平移观察点到世界坐标系的原点
	2) 进行旋转变换，使得观察坐标轴与世界坐标轴重合
4、什么叫正交投影？你认为工程上使用正交投影的主要理由是？正交投影是平行投影，投影线和投影面夹角90°，投影面为坐标平面，能更好反应尺寸
正投影与轴侧投影的区别？投影面是倾斜的
5、平行投影与透视投影有哪些本质区别？斜平行投影与正交投影的主要区别？
6、 试用立方体为例画图描述透视投影近大远小，以及与投影面平行的平行直线透视投影后仍然平行，空间相交的直线透视投影后仍然平行，不与投影面平行的平行直线透视投影后汇聚成一点。
7、用图描述：1）透视投影的近大远小，2）平行投影能够准确反映物体尺寸，3）正平行投影和斜平行投影的区别。
![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240609165232.png)
- 什么叫灭点？什么叫投影中心？什么叫投影点？
	- 
- 正平行投影变换函数？ **编程题**  
- 透视投影变换函数？如何调整？
- 观察点函数的设置？需要注意什么？
- 画图描述正平投影、斜平行投影区别
- 画图描述透视投影的特性
![image.png|425](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610111705.png)

# chap 6 三维造型
重点放在曲线曲面造型
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610001126.png)
- 规则曲线曲面-Regular
	初等解析曲面：平面、圆柱面、圆锥面、球面、圆环面
	二次曲面：球面、椭圆面、环面、抛物面和双曲面
- 自由曲线曲面-Free
### 曲线曲面表示方法
- 显式表示
$$y=f(x), z=g(x,y)$$
一般一个x与一个y对应，但是这样不能表示封闭或者多值曲线(如圆)
- 隐式表示
$$F(x,y)=0, x^2 + y^2 = r^2$$
易于判断函数是否大于0，但是可能存在多值问题，即多组函数自变量对应同一个函数值
- 常用：**参数方程**
$$x = x(t), y = y(t)$$
$$C(t) = [x,y,z] = [x(t),y(t),z(t)]$$
任意空间曲面可表示为有两个参数的参数方程：$$x = x(u,v), y = y(u,v)$$$$S(u,v)=[x,y,z]=[x(u,v),y(u,v),z(u,v)]
$$
其中，u,v起始均为0，终止为1
### 插值与逼近
插值：点点通过
逼近：尽量靠近
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610002523.png)
型值点：通过计算得到**曲线曲面上的点**
控制点：**控制曲线曲面形状**的点
#### 参数连续性和几何连续性
##### 参数连续性C
![image.png|525](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610002745.png)
##### 几何连续性G
![image.png|525](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610002821.png)

在曲线曲面参数表示中，一般采用控制点和基函数形式：
$$C(t)=\sum_{t=1}^n P_iB_i(t)$$
其中$P_i$为控制点，$B_i(t)$为基函数
一般采用3次多项式，因为可以达到2阶导数连续
#### Bezier曲线
前四个性质非常重要！
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610003253.png)
- 局限性：
	- 控制点的数目决定曲线的阶次,n+1个顶点产生n次曲线
	- 没有局部性
	- 逼近的方法
#### B样条
- 保留了Bezier方法的优点，克服了Bezier方法的一些局限性
- 曲线的阶数不随控制点增加而增加
- 具有**局部性**
- 但仍然是一种逼近的方法 **不具备插值性**！
- 二阶导数连续
- **凸包性**
#### NURBS曲线
- **统一表达自由曲线曲面和二次曲线曲面**
- 保留了B样条函数的基本性质
#### 多结点样条 Many-knot spline
- 由B样条线性组合而来，继承了一般样条函数的优点，如连续性，对称性和规范性。
- 同时又具备局部性、**插值性**、显式不求解特性
- 不具有凸包性
![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610003752.png)

1. 试列举至少4种构造曲线曲面的方法，并比较它们的相同与不同 
2. 曲线表示法有哪些种？试分别举例
3. 什么是曲线的凸包性，是举例哪些曲线具有凸包性？ 
	- 凸包性：曲线位于控制点构成的特征多边形凸包内，Bezier、B样条、NURBS均具有
4. Bezier 曲线的性质有哪些性质？
	- 凸包性、几何不变性、光滑性、**端点特性、切矢特性**
5. 已知四个控制点P0，P1，P2，P3，画出曲线的形状并标注坐标、控制点和曲线端点切矢的方向。

![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610004102.png)
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610004112.png)
两端相邻曲线$Ci(t)$和$C_{i+1}(t)$,若在连接点P处满足$C_i(t)=C_{i+1}(t)$,则表示两端曲线在连接点P处位置连续。若两端曲线在满足以上条件的基础上，还满足条件**一阶导数相等**，则表示两端曲线是$C_1$连续的。如果还满足条件**二阶导数相等**，则表示两端曲线是$C_2$连续的。

$n$次Bezier曲线至少有**n+1**个控制点。如果100个控制点要构成一段3次Bezier光滑曲线，则必须对这些控制点进行分组分段拼接曲线, 且**每4个控制点**为一组，分段曲线在连接处至少保持**二阶导数连续**。基函数为$3次B样条$的一段光滑曲线有$100$个控制点，整段曲线的次数为**3**。

# chap 7 真实感图形绘制
![image.png|500](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610100509.png)
- 物体表面的精确表示
- 场景中光照效果的物理描述
### 颜色模型
见上
### 消隐技术
考这个。图像空间的消隐算法，对屏幕上每一个像素进行判断
![image.png|525](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610100930.png)
1. 将深度缓存与帧缓存中的所有单元(x,y)初始化，使得`depthBuff(x,y)=1.0， frameBuff(x,y)=backgroundColor`
2. 处理场景中的每一个多边形，每次一个。计算多边形面上**各点(x,y)处的深度值z**（如果不是已知）
	若`z<depthBuff(x,y)`，则计算该位置的表面颜色并设定`depthBuff(x,y)=z, frameBuff(x,y)=surfaceColor`
	当处理完所有多边形面后，深度缓存中保存的是**可见面的深度值**，而**帧缓存保存了这些表面的对应属性值**。
![image.png|525](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610103226.png)
### 光照模型
- 环境光Ambient Light
	物体表面反射环境光的结果
- 漫反射 Diffuse reflection
	粗糙或颗粒状表面向各方向反射光
- 镜面反射 Specular reflection
	光滑表面会集中一个方向反射光，形成一个亮点
#### 环境光
环境光反射光的强度：
$$I_e = I_a \cdot K_a$$
$I_a$表示环境光强度，环境光对物体的反射光**与观察方向无关**，**与物体表面的空间方向**无关
$K_a$为物体对环境光的反射系数，$0 \le K_a \le1$
#### 漫反射
理想漫反射体从物体表面各个方向**等强度反射光线**，**与观察方向无关**（与视点位置无关）
表面漫反射光强度$$I_d = I_p K_d cosθ$$
$I_p$ —入射光的强度（点光源）
$K_d$ — 入射光的漫反射系数($0< K_d <1$),由物体表面的材料性质以及入射光的波长所决定。
$θ$ — 入射光与表面上点的**法向量**N之间的夹角
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610104627.png)
#### 镜面反射
某个观察方向可以看到高光或亮点
$$I_s = I_pK_scos^n\alpha$$
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610105036.png)
#### Phong光照明模型
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610105449.png)
### 光线追踪
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610105644.png)
光线跟踪终止条件：
1）未碰到任何物体
2）碰到背景
3）光强很弱
4）深度很深
### 纹理映射
纹理空间$p(s,t)$，水平方向为$s$方向，垂直方向为$t$方向，取值范围为$(0,1)$
纹理空间的任一纹元$P(s,t)$与世界坐标系中几何图形的点$P(x,y,z)$对应，有$$s = f(x,y,z),t=g(x,y,z)$$
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610110202.png)
![image.png|550](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240610110317.png)
