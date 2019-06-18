[TOC]

### FAST角点检测

> Features from Accelerated Segment Test

#### 一、基本算法原理

FAST角点定义为：若某像素点与其周围领域内足够多的像素点处于不同的区域，则该像素点可能为角点。也就是某些属性与众不同，考虑灰度图像，即若该点的灰度值比其周围领域内足够多的像素点的灰度值大或者小，则该点可能为角点。

#### 二、FAST算法流程

`step1`: 从图片中选取一个像素$P$，设其亮度(灰度)$I_p$

`step2`: 设定一个合适的阈值

`step3`: 考虑以该像素点为中心的一个半径等于3像素的离散化的Bresenham圆，这个圆的边界上有16个像素（如图1所示）。

![](figs/fast.jpg)

`step4`: 现在，如果在这个大小为16个像素的圆上有nn个连续的像素点，它们的像素值要么都比$I_p+t$大，要么都比$I_p−t$小，那么它就是一个角点。（如图1中的白色虚线所示）。$n$的值可以设置为12或者9，实验证明选择9可能会有更好的效果。



#### 三、非极大值抑制

从邻近的位置选取了多个特征点是另一个问题，我们可以使用Non-Maximal Suppression来解决。

1. 为每一个检测到的特征点计算它的响应大小（score function）$V$。这里$V$定义为点$p$和它周围16个像素点的绝对偏差的和。
2. 考虑两个相邻的特征点，并比较它们的$V$值。
3. $V$值较低的点将会被删除。



#### 四、OpenCV中的接口

```c++
void FAST(InputArray image, vector<KeyPoint>& keypoints, int threshold, bool nonmaxSuppression=true )

C++: void FASTX(InputArray image, vector<KeyPoint>& keypoints, int threshold, bool nonmaxSuppression, int type)
```

