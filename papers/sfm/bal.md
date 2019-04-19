## Bundle Adjustment in the Large 
`Date:2019/04/10`
>KeyWords: `BundleAdjustment` `SfM` `Nonlinear Least Squares`

### Definitions of BundleAdjustment
Given a set of measured image feature locations and correspondences, the goal
of bundle adjustment is to find 3D point positions and camera parameters that
minimize the reprojection error.

### Levenberg Marquardt Algorithm
#### 问题描述
假设$x\in \mathbb{R}^n$为$n$维向量，$F(x)=[f_1(x),f_2(x),\cdots,f_m(x)]^T$
为关于$x$的$n$维向量函数，该优化问题可以定义成如下形式:
$$
\min_x\frac{1}{2}\Vert F(x) \Vert^2
$$
函数$F(x)$的$Jacobian$矩阵为一个$m\times n$的矩阵，其中$J_{ij}(x)=\partial_jf_i(x)$
函数$F(x)$的梯度向量为：$g(x)=\nabla \frac{1}{2}\Vert F(x) \Vert^2=J(x)^TF(x)$


#### 解法
通常求解非线性优化问题一般都是通过求解对原问题的一些列近似来完成，对于非线性最小二乘问题，近似可以通过线性化来完成：
$$
F(x+\Delta x) \approx F(x)+J(x)\Delta x
$$
于是便可以导出如下形式的问题：
$$
\min_{\Delta x}\frac{1}{2}\Vert J(x)\Delta x + F(x) \Vert^2
$$
每一次迭代都会在原有的$x$基础上求解一个$\Delta x$，但是通常$x\leftarrow x+\Delta x$不一定会收敛。所以需要对步长$\Delta x$的大小进行控制。一种方法是为其引入正则项：
$$
\min_{\Delta x}\frac{1}{2}\Vert J(x)\Delta x + F(x) \Vert ^2 + \mu \Vert D(x)\Delta x\Vert^2
其中$D(x)$为一个非负的对角阵，通常对角元素为矩阵$J(x)^TJ(x)$对角元素的平方根，$\mu$为非负参数用来控制正则化的强度。
