[TOC]

### 张正友标定法

#### 相机内参数

场景点$P=(X,Y,Z)$，二维图像点$p=(u,v)$
$$
s\left(\begin{array}{c}\mu\\ \nu \\ 1\end{array}\right) = \left[\begin{array}{ccc}\alpha & 0 &c_x \\ 0 & \beta & c_y \\ 0 & 0 & 1\end{array}\right] \left[\begin{array}{cccc}f&0&0&0\\0&f&0&0\\0&0&1&0\end{array}\right] \left[\begin{array}{cc}R&t\\0^T&1\end{array}\right] \left(\begin{array}{c}X\\Y\\Z\\1\end{array}\right) \\
= \left[\begin{array}{cccC}f_x&0&c_x&0\\0&f_y&c_y&0\\0&0&1&0\end{array}\right]\left[\begin{array}{cc}R&t\\0^T&1\end{array}\right] \left(\begin{array}{c}X\\Y\\Z\\1\end{array}\right)
$$
矩阵$K$称为相机内参：
$$
K = \left[\begin{array}{ccc}f_x&0&c_x\\0&f_y&c_y\\0&0&1\end{array}\right]
$$
其中$\alpha,\beta $表示单位距离上像素的个数，则:$f_x= \alpha f,f_y =\beta f$ ，外加一个扭曲参数$\gamma$ 可以得到：
$$
K = \left[\begin{array}{ccc}f_x&\gamma&c_x\\0&f_y&c_y\\0&0&1\end{array}\right]
$$
大多数相机可以把$\gamma$设置为0

#### 张正友标定法

棋盘格坐标为已知量，所以已知棋盘格所在平面$\Pi $和棋盘格在相机成像平面的像为另一个平面$\pi$，知道了两个平面对应点的坐标可以求得两个平面间的$H$矩阵(棋盘格可以通过角点提取算法获取点坐标)。

成像模型:
$$
p = K[R|t]P 
$$
可以得到：
$$
H = K[R|t]
$$
假设棋盘格所在平面为世界坐标系中$Z=0$的平面，棋盘格上任意点坐标为$P=(X,Y,0)$ ，利用成像模型：
$$
s\left(
\begin{array}{c} u \\ v \\1 \end{array}  \right) = K\left[\begin{array}{c}R&t\end{array}\right]\left(
\begin{array}{c} X \\ Y \\ 0 \\ 1 \end{array}  \right) = K[\begin{array}{c}r_1&r_2&r_3&t\end{array}]\left(\begin{array}{c} X \\ Y \\ 0 \\ 1 \end{array}  \right)
= K[\begin{array}{c}r_1&r_2&t\end{array}]\left(\begin{array}{c} X \\ Y \\ 1 \end{array}  \right)
$$

$$
s\left(
\begin{array}{c} u \\ v \\1 \end{array}  \right) = H \left(\begin{array}{c} X \\ Y \\ 1 \end{array}  \right)
$$



于是可以得到：
$$
H =\left[\begin{array}{c}h_1&h_2&h_3\end{array}\right]= \lambda K[\begin{array}{c}r_1&r_2&t\end{array}]
$$
把$R,t$利用$H$表示出来：
$$
\begin{array}{l}
r_1 = \lambda K^{-1}h_1 \\
r_2 = \lambda K^{-1}h_2 \\
t = \lambda K^{-1}h_3
\end{array}
$$
利用旋转矩阵性质：
$$
\begin{array}{l}
r_1^Tr_2 = 0 \\
\Arrowvert r_1 \Arrowvert = \Arrowvert r_2 \Arrowvert = 1 \\
\end{array}
$$
于是一张棋盘格图像可以获得两个约束：
$$
\left\{
\begin{array}{l}h_1^TK^{-T}K^{-1}h_2 = 0 \\ h_1^TK^{-T}K^{-1}h_1 = h_2^TK^{-T}K^{-1}h_2 = 1\end{array}
\right.
$$

#### 求解内参

令：
$$
B=K^{-T}K^{-1}=
\left[
\begin{array}{ccc}
B_{11} & B_{12} & B_{13} \\
B_{21} & B_{22} & B_{23} \\
B_{31} & B_{32} & B_{33}
\end{array}
\right] \\=
\left[
\begin{array}{ccc}
\frac{1}{\alpha^2} & -\frac{\gamma}{\alpha^2\beta} & \frac{v_0\gamma-u_0\beta}{\alpha^2\beta} \\
-\frac{\gamma}{\alpha^2\beta} & \frac{\gamma^2}{\alpha^2\beta^2}+\frac{1}{\beta^2} & -\frac{\gamma(v_0\gamma-u_0\beta)}{\alpha^2\beta^2}-\frac{v_0}{\beta^2} \\
\frac{v_0\gamma-u_0\beta}{\alpha^2\beta} & -\frac{\gamma(v_0\gamma-u_0\beta)}{\alpha^2\beta^2}-\frac{v_0}{\beta^2} & \frac{(v_0\gamma-u_0\beta)^2}{\alpha^2\beta^2}+\frac{v_0}{\beta^2}+1
\end{array}
\right]
$$
$B$为对称矩阵，未知量有6个：
$$
b =\left[\begin{array}{ccc}B_{11} ,B_{12} , B_{22},B_{13},B_{23},B_{33}\end{array}\right]
$$
令$h_i$为单应矩阵$H$的第$i$个行向量，则有：
$$
h_i = [h_{i1},h_{i2},h_{i3}]^T
$$
故：
$$
h_iK^{-T}K^{-1}h_j = h_iBh_j = v_{ij}^Tb\\其中，v_{ij}=\left[ \begin{array}{cccccc} h_{i1}h_{j1} & h_{i1}h_{j2}+h_{i2}h_{j1} & h_{i2}h_{j2} & h_{i3}h_{j1}+h_{i1}h_{j3} & h_{i3}h_{j2}+h_{i2}h_{j3} & h_{i3}h_{j3} \end{array} \right]^T
$$
约束方程：
$$
\left\{
\begin{array}{l}h_1^TK^{-T}K^{-1}h_2 = 0 \\ h_1^TK^{-T}K^{-1}h_1 = h_2^TK^{-T}K^{-1}h_2 = 1\end{array}
\right. \Rightarrow \left\{ \begin{array}{l}v_{22}^Tb = 0 \\ v_{11}b = v_{12}b \end{array}\right.
$$
矩阵形式：
$$
\left[ \begin{array}{c} v_{12}^T \\ v_{11}-v_{22} \end{array}\right] b  = 0
$$
假设有$n$幅图像，则：
$$
Vb = 0
$$
其中$V$是一个$2n\times 6$的矩阵，$b$为6维向量，故：

+ 当$n\ge 3$，可以得到$b$的唯一解
+ 当$n=2$ ，则可以假设扭曲参数$\gamma=0$ 作为额外约束
+ 当$n=1$，则只能计算相机的两个参数

对于方程$Vb=0$可以利用SVD求其最小二乘解，对$V^TV$进行SVD分解，最小二乘解为最小特征值对应的特征向量。结果的B相差一个尺度$\lambda$
$$
B= \lambda A^{-T}A
$$
可以得到相机的各个参数：
$$
\left\{
\begin{array}{l}
c_x = \gamma c_y /\beta - B_{13}\alpha^2 / \lambda  \\
c_y = (B_{12}B_{13}-B_{11}B_{23})/(B_{11}B_{22}-B_{12}^2) \\
\alpha = \sqrt{\lambda /B_{11}} \\
\beta = \sqrt{\lambda B_{11}/(B_{11}B_{22}-B_{12}^2)} \\
\gamma = -B_{12}\alpha^2\beta / \lambda \\
\lambda = B_{33}-[B_{13}^2 + c_y(B_{12}B_{13}-B_{11}B_{23})]/B_{11}
\end{array}
\right.
$$

#### 最大似然估计

最小二乘得到的解没有实际物理意义，可以利用最大似然估计优化解。

假设统一相机从$n$个不同视角获得了$n$幅标定板图像，每一幅图像上有$m$个像点。

$M_{ij}$表示第$i$幅图像上第$j$个像点对应标定板上的三维点，则：
$$
\hat{m}(K,R_i,t_i,M_{ij}) = K[\begin{array}{c}R&t\end{array}]M_{ij}
$$
像点$m_{ij}$的概率密度函数为：
$$
f(m_{ij})=\frac{1}{\sqrt{2\pi}}e^{\frac{-(\hat{m}(K,R_i,t_i,M_{ij})-m_{ij})^2}{\sigma^2}}
$$
构造似然函数：
$$
L(K,R_i,t_i,M_{ij}) = \prod^{n,m}_{i=1,j=1}f(m_{ij})=\frac{1}{\sqrt{2\pi}}e^{\frac{-\sum^n_{i=1}\sum^m_{j=1}(\hat{m}(K,R_i,t_i,M_{ij})-m_{ij})^2}{\sigma^2}}
$$
即优化如下目标函数：
$$
\arg \min_{K,R_i,t_i,M_{ij}}\sum^n_{i=1}\sum^m_{j=1} \| \hat{m}(K,R_i,t_i,M_{ij})-m_{ij} \|^2
$$

> 本质就是BundleAdjustment最小化重投影误差

#### 畸变影响

设$(u,v)$为无畸变像素坐标，$(\hat{u},\hat{v})$为畸变作用后的像素坐标，$(u_0,v_0)$为相机主点。

$(x,y)$和$(\hat{x},\hat{y})$分别为畸变前后归一化图像坐标，即：
$$
\hat{x} = x + x[k_1(x^2 + y^2) + k_2(x^2 + y^2)^2] \\
\hat{y} = y + y[k_1(x^2 + y^2) + k_2(x^2 + y^2)^2]
$$

$$
\begin{array}{l}
\hat{u} = u_0 + \alpha \hat{x} + \gamma \hat{y} \\
\hat{v} = v_0 + \beta \hat{y} \\
\end{array}
$$

假设$\gamma=0$ ，则有：
$$
\hat u = u + (u-u_0)[k_1(x^2+y^2)+k_2(x^2+y^2)^2] \\
\hat v = v + (v-v_0)[k_1(x^2+y^2)+k_2(x^2+y^2)^2]
$$
写成矩阵形式：
$$
\left[
\begin{array}{cc}
(u-u_0)(x^2+y^2) & (u-u_0)(x^2+y^2)^2 \\
(v-v_0)(x^2+y^2) & (v-v_0)(x^2+y^2)^2
\end{array}
\right]
\left[
\begin{array}{c}
k_1 \\ k_2
\end{array}
\right]=
\left[
\begin{array}{c}
\hat u-u \\ \hat v -v
\end{array}
\right]
$$
假设有$n$幅图像，每幅图像有$m$个点，可以得到$2mn$个等式，写成矩阵形式：
$$
Dk=d
$$
可以得到:
$$
k=[k_1\ k_2]^T = (D^TD)^{-1}D^Td
$$
再次利用优化算法进行优化即可：
$$
\sum^n_{i=1}\sum^m_{j=1} \| \hat{m}(K,k_1,k_2,R_i,t_i,M_{ij})-m_{ij} \|^2
$$

> 可以利用去畸变之后的图像再次计算相机内参



#### 参考资料

[ref](https://blog.csdn.net/brookicv/article/details/79155013)