### 舒尔补 (Schur Complement)

#### 定义

矩阵$A,B,C,D$分别为$p\times p$ , $p\times q$, $q \times p$, $q\times q$ 的矩阵块，一起构成矩阵:
$$
M = \begin{bmatrix}
A  & B \\
C & D 
\end{bmatrix}
$$
A,D可逆,则：

- D相对于M的Schur Complement为： $A-BD^{-1}C$
- A相对于M的Schur Complement为： $D - CA^{-1}B$

#### 背景知识

给定变换矩阵:
$$
L=\left[ \begin{array}{cc}{I_{p}} & {0} \\ {-D^{-1} C} & {I_{q}}\end{array}\right] \\
L^{-1}=\left[ \begin{array}{cc}{I_{p}} & {0} \\ {D^{-1} C} & {I_{q}}\end{array}\right]
$$

$$
\begin{aligned} M L &=\left[ \begin{array}{cc}{A} & {B} \\ {C} & {D}\end{array}\right] \left[ \begin{array}{cc}{I_{p}} & {0} \\ {-D^{-1} C} & {I_{q}}\end{array}\right]=\left[ \begin{array}{cc}{A-B D^{-1} C} & {B} \\ {0} & {D}\end{array}\right] \\ &=\left[ \begin{array}{cc}{I_{p}} & {B D^{-1}} \\ {0} & {I_{q}}\end{array}\right] \left[ \begin{array}{cc}{A-B D^{-1} C} & {0} \\ {0} & {D}\end{array}\right] \end{aligned}
$$
于是：
$$
\left[ \begin{array}{ll}{A} & {B} \\ {C} & {D}\end{array}\right]=\left[ \begin{array}{cc}{I_{p}} & {B D^{-1}} \\ {0} & {I_{q}}\end{array}\right] \left[ \begin{array}{cc}{A-B D^{-1} C} & {0} \\ {0} & {D}\end{array}\right] \left[ \begin{array}{cc}{I_{p}} & {0} \\ {D^{-1} C} & {I_{q}}\end{array}\right]
$$
与LDU分解十分相似。
$$
M^{-1}=\left[ \begin{array}{cc}{A^{-1}+A^{-1} B(M / A)^{-1} C A^{-1}} & {-A^{-1} B(M / A)^{-1}} \\ {-(M / A)^{-1} C A^{-1}} & {(M / A)^{-1}}\end{array}\right]
$$


#### 应用

##### 求解线性方程组：

$$
\begin{array}{l}{A x+B y=a} \\ {C x+D y=b}\end{array}
$$

对于第二个方程，两边同时乘以$BD^{-1}$再用第一个方程减去它可以得到：
$$
\left(A-B D^{-1} C\right) x=a-B D^{-1} b
$$
即先求解$x$再求解$y$，将原本$(p+q)\times (p+q)$大小的矩阵求逆问题简化为:$p\times p$和$q\times q$矩阵的求逆问题。

##### 概率中的应用

向量$X\in \mathbb{R}^n,Y\in \mathbb{R}^m$ ，$(X,Y)\in \mathbb{R}^{n+m}$服从多维正态分布，协方差矩阵为：
$$
\Sigma=\left[ \begin{array}{ll}{A} & {B} \\ {B^{\top}} & {C}\end{array}\right]
$$
$A\in \mathbb{R}^{n\times n}$为$X$的协方差矩阵,$C\in \mathbb{R}^{m\times m}$为Y的协方差矩阵，$B\in \mathbb{R}^{n\times m}$为X,Y之间的协方差矩阵。于是：
$$
\begin{aligned} \operatorname{Cov}(X | Y) &=A-B C^{-1} B^{\top} \\ & \mathrm{E}(X | Y)=\mathrm{E}(X)+B C^{-1}(Y-\mathrm{E}(Y)) \end{aligned}
$$


