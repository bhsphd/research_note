### 仿射变换 WarpAffine

### 理论部分
#### 什么是仿射变换?
>可以看成是线性变化和平移的组合。

#### 仿射变换可以表示：
+ 旋转变换(Rotation)
+ 平移(Translation)
+ 尺度变换(Scale)

#### 公式推导
通常仿射变换可以用一个$2\times3$的矩阵来表示:
$$
A={
\begin{bmatrix}
a_{00} & a_{01} \\
a_{10} & a_{11}
\end{bmatrix}
}_{2\times2},
B={
\begin{bmatrix}
b_{00} \\
a_{10}
\end{bmatrix}
}_{2\times1}
$$
$$
M=\left[A\quad B\right]=
{
\begin{bmatrix}
a_{00} & a_{01} &b_{00}\\
a_{10} & a_{11} &b_{10}
\end{bmatrix}
}_{2\times3}
$$
如果我们想要对一个$2D$向量 $\begin{bmatrix} x\\ y \end{bmatrix}$ ，用$A,B$进行变换，可以写成:
$$
T = A \cdot  \begin{bmatrix} x\\ y \end{bmatrix} +B
$$
或者是：
$$
T=M\cdot [x,y,1]^T
$$
于是：
$$
T=\begin{bmatrix}
a_{00}x+a_{01}y+b_{00} \\
a_{10}x+a_{11}y+b_{10}
\end{bmatrix}
$$


#### 常见的仿射变换
>对某个向量进行变换

##### 平移变换
$$
\begin{bmatrix}
1 &0 &t_x \\
0 &1 &t_y \\
0 &0 &1  
\end{bmatrix}
$$

##### 镜像
$$
\begin{bmatrix}
-1 &0 &0 \\
0 &1 &0 \\
0 &0 &1  
\end{bmatrix}
$$

##### 缩放变换
$$
\begin{bmatrix}
c_x=2 &0 &0 \\
0 &c_y=1 &0 \\
0 &0 &1  
\end{bmatrix}
$$

##### 旋转
$$
\begin{bmatrix}
cos(\theta) &-sin(\theta) &0 \\
sin(\theta) &cos(\theta) &0 \\
0 &0 &1  
\end{bmatrix}
$$

##### 剪切
$$
\begin{bmatrix}
1 &c_x=0.5 &0 \\
c_y=0 &1 &0 \\
0 &0 &1  
\end{bmatrix}
$$

#### 仿射变换的求解
至少需要 3 对$2D-2D$点对应才能够求解。


#### 绕指定点的旋转
如果是绕指定坐标点进行的旋转，则可以表示为多个变换的组合:
$$
A=\begin{bmatrix}
1 &0 &-a \\
0 &1 &-b \\
0 &0 &1  
\end{bmatrix}
$$
$$
B=\begin{bmatrix}
cos(\theta) &-sin(\theta) &0 \\
sin(\theta) &cos(\theta) &0 \\
0 &0 &1  
\end{bmatrix}
$$

$$
C=\begin{bmatrix}
1 &0 &a \\
0 &1 &b \\
0 &0 &1  
\end{bmatrix}
$$
首先利用变换$A$，将坐标变换到 $(a,b)$ 处，再进行旋转变换 $B$ ,之后再将坐标系平移回原点，即变换$C$,总的变换矩阵为：
$$
T=C\cdot B \cdot A
$$
