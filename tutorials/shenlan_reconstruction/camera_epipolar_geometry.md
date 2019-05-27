[TOC]
### 针孔相机模型
#### 相机外参
![](./figs/extrinsics.png)

常用的公式：

+ 相机中心在世界坐标系中的位置：$\boldsymbol{O}_{\mathrm{can}}^{w}=\boldsymbol{R}^{T} \boldsymbol{O}_{\mathrm{cam}}^{c}-\boldsymbol{R}^{T} \boldsymbol{t}=-\boldsymbol{R}^{T} \boldsymbol{t}$
+ 相机朝向(Z轴)在世界坐标系中的方向：$\boldsymbol{r}^w = \boldsymbol{R}(2,:)$ 

![](./figs/normalize_plane.png)

![](./figs/pinhole_model.png)

![](./figs/pinhole_model2.png)

![](./figs/pinhole_model3.png)

#### 畸变模型

![](./figs/distort1.png)

![](./figs/distort2.png)

![](./figs/distort3.png)

### 对极几何