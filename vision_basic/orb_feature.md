[TOC]

### ORB特征点提取与匹配

#### 一、基本原理

ORB特征是将FAST特征点的检测方法与BRIEF特征描述子结合起来，并在它们原来的基础上做了改进与优化。

首先，它利用FAST特征点检测的方法来检测特征点，然后利用Harris角点的度量方法，从FAST特征点从挑选出Harris角点响应值最大的NN个特征点。其中Harris角点的响应函数定义为：		
$$
R=det \boldsymbol{M} - \alpha(trace\boldsymbol{M})^2
$$

#### 二、旋转不变性

我们知道FAST特征点是没有尺度不变性的，所以我们可以通过构建高斯金字塔，然后在每一层金字塔图像上检测角点，来实现尺度不变性。那么，对于局部不变性，我们还差一个问题没有解决，就是FAST特征点不具有方向，ORB的论文中提出了一种利用灰度质心法来解决这个问题，灰度质心法假设角点的灰度与质心之间存在一个偏移，这个向量可以用于表示一个方向。对于任意一个特征点$p$来说，我们定义$p$的邻域像素的矩为：
$$
m_{pq} = \sum_{x,y}x^py^qI(x,y)
$$
其中$I(x,y)$为点$(x,y)$处的灰度值，可以得到图像的质心为：
$$
C=(\frac{m_{10}}{m_{00}},\frac{m_{10}}{m_{00}})
$$
特征点与质心的夹角定义为FAST特征点的方向：
$$
\theta = arctan(m_{01},m_{10})
$$
为了提高方法的旋转不变性，需要确保$x$和$y$在半径为$r$的圆形区域内，即$x,y\in[-r,r]$



#### 三、特征点的描述

ORB选择了BRIEF作为特征描述子，由于BRIEF没有旋转不变性，所以需要给BRIEF加上旋转不变性，称为:Steer BRIEF。对于任何一个特征点来说，它的BRIEF描述子是一个长度为$n$的二进制串，由其周围$n$个点对(2$n$个点)生成，将这$2n$个点$(x_i,y_i)$组成一个矩阵$S$：
$$
S =\begin{pmatrix}x_1&x_2&\cdots&x_{2n}\\y_1&y_2&\cdots&y_{2n}\end{pmatrix}
$$


ORB中采用的是使用邻域方向$\theta$和对应的旋转矩阵$R_{\theta}$，对$S$进行变换，构建成$S_{\theta}$，

其中:
$$
R_{\theta} = \begin{bmatrix}cos\theta & sin\theta \\ –sin\theta &cos\theta\end{bmatrix}
$$

由于$S$坐标固定，可以通过对$360^o$进行离散化，构建查找表，快速求得点集$S_{\theta}$。



#### 四、特征描述子的区分性

点对的选取策略，为了保证方差较大，更容易区分

[orb](https://www.cnblogs.com/ronny/p/4083537.html)

#### 五、OpenCV中的ORB特征

```c++
ORB::ORB(int nfeatures=500, float scaleFactor=1.2f, int nlevels=8, int edgeThreshold=31, int firstLevel=0, int WTA_K=2, int scoreType=ORB::HARRIS_SCORE, int patchSize=31)
```

`nfeatures`: 最多提取的特征点的数量

`scaleFactor`: 金字塔图像之间的尺度参数，类似于SIFT中的$k$

`nlevels`: 高斯金字塔的层数

`edgeThreshold`: 边缘阈值，这个值主要是根据后面的patchSize来定的，靠近边缘edgeThreshold以内的像素是不检测特征点的。

`firstLevel`: 看过SIFT都知道，我们可以指定第一层的索引值，这里默认为0。

`WET_K`: 用于产生BIREF描述子的 点对的个数，一般为2个，也可以设置为3个或4个，那么这时候描述子之间的距离计算就不能用汉明距离了，而是应该用一个变种。OpenCV中，如果设置WET_K = 2，则选用点对就只有2个点，匹配的时候距离参数选择NORM_HAMMING，如果WET_K设置为3或4，则BIREF描述子会选择3个或4个点，那么后面匹配的时候应该选择的距离参数为NORM_HAMMING2。

`scoreType`: 用于对特征点进行排序的算法，你可以选择HARRIS_SCORE，也可以选择FAST_SCORE，但是它也只是比前者快一点点而已。

`patchSize`: 用于计算BIREF描述子的特征点邻域大小。