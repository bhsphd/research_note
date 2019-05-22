### 特征点提取与匹配

#### 特征点介绍

##### Harris角点检测

特征点具有局部差异性，定义：
$$
\begin{aligned}
E(u, v) &=\sum_{(x, y)} w(x, y)[I(x+u, y+v)-I(x, y)]^{2} \\
& \approx  \sum_{(x,y)} w(x,y) [I(x,y)+\frac{\partial I}{\partial x}(x, y) u+\frac{\partial I}{\partial y}(x, y) v - I(x,y)]^2 \\
&= \begin{bmatrix} u \\v \end{bmatrix}^T \mathbf{H} \begin{bmatrix} u \\v \end{bmatrix} 
\end{aligned}
$$
$(u,v)$为整个窗口移动的距离，$(x,y)$为窗口内点坐标$w(x,y)$为权重。

图像梯度:$\nabla I(x, y)=\left(\frac{\partial I}{\partial x}(x, y), \frac{\partial I}{\partial y}(x, y)\right)$ 

Harris矩阵:$\boldsymbol{H}=\left[ \begin{array}{cc}{\sum_{(x, y)} w(x, y)\left(\frac{\partial I}{\partial x}(x, y)\right)^{2}} & {\sum_{(x, y)} w(x, y)\left(\frac{\partial I}{\partial x}(x, y) \frac{\partial I}{\partial y}(x, y)\right)} \\ {\sum_{(x, y)} w(x, y)\left(\frac{\partial I}{\partial x}(x, y) \frac{\partial I}{\partial y}(x, y)\right)} & {\sum_{(x, y)} w(x, y)\left(\frac{\partial I}{\partial y}(x, y)\right)^{2}}\end{array}\right]$

根据$Harris$矩阵的特征值分析：

$S V D(\boldsymbol{H})=\boldsymbol{U} \sum \boldsymbol{V}, \quad\left(\lambda_{1}, \lambda_{2}\right), \lambda_{1}>\lambda_{2}$ 

+ $\lambda _1\approx \lambda_2 \approx0$ : 兴趣点位于光滑区域
+ $\lambda _1 > 0 , \lambda_2 \approx0$：兴趣点位于边缘区域
+ $\lambda _1> 0, \lambda_2 > 0$：兴趣点位于角点区域

`判定准则`
$$
C  =det(H)-ktrace(H)^2 = \lambda _1 \lambda _2 - k(\lambda _1+\lambda _2 )^2,k=0.04 
$$

+ k的值越小，越敏感
+ 只有当$\lambda_1$ 和$\lambda_2$同时取得最大值的时候，$C$才能取得较大值
+ 避免了特征值分解，提高计算效率

`非极大值抑制(Non-maximal Suppression)`

选取局部响应最大值，避免重复检测

`算法流程`

计算水平和垂直方向梯度:
$$
\frac{\partial L\left(x, y, \sigma_{D}\right)}{\partial x}=\frac{\partial G\left(x, y, \sigma_{D}\right)}{\partial x} * I(x, y), \frac{\partial L\left(x, y, \sigma_{D}\right)}{\partial y}=\frac{\partial G\left(x, y, \sigma_{D}\right)}{\partial y} * I(x, y)
$$
计算每个像素位置的$Harris$矩阵:
$$
\boldsymbol{H}=G\left(x, y, \sigma_{I}\right) * \left[ \begin{array}{cc}{\left(\frac{\partial L\left(x, y, \sigma_{D}\right)}{\partial x}(x, y)\right)^{2}} & {\frac{\partial L\left(x, y, \sigma_{D}\right)}{\partial x}(x, y) \frac{\partial L\left(x, y, \sigma_{D}\right)}{\partial y}(x, y)} \\ {\frac{\partial L\left(x, y, \sigma_{D}\right)}{\partial x}(x, y) \frac{\partial L\left(x, y, \sigma_{D}\right)}{\partial y}(x, y)} & {\left(\frac{\partial L\left(x, y, \sigma_{D}\right)}{\partial y}(x, y)\right)^{2}}\end{array}\right.
$$
计算每个像素位置的$Harris$角点响应值：
$$
C=\operatorname{det}(H)-k \operatorname{trace}(H)^{2}=\lambda_{1} \lambda_{2}-k\left(\lambda_{1}+\lambda_{2}\right)^{2}
$$
找到$Harris$角点响应值大于给定阈值且局部最大的位置作为特征点。
