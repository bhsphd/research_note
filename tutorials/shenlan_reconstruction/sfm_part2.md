[TOC]

### 双视角SfM

#### 常见BA形式

##### 数学模型

![](./figs/sfm_ba1.png)

![](./figs/sfm_ba2.png)

![](./figs/sfm_ba3.png)

![](./figs/sfm_ba4.png)

![](./figs/sfm_ba5.png)

![](./figs/sfm_ba6.png)

![](./figs/sfm_ba7.png)

![](./figs/sfm_ba8.png)

![](./figs/sfm_ba9.png)

![](./figs/sfm_ba10.png)



#### Tricks

##### 算法整体流程

![](./figs/sfm_tricks.png)



##### 初始图像对选取

+ 匹配点足够多(>50)
+ 基线足够长(三角测量角足够大>5度)
+ 满足Homography的匹配尽量少(内点<60%)
+ 成功三角化的匹配对>50%

##### Tracks 滤波

+ 过滤三维空间中太远的点
+ 去除重投影误差较大的点