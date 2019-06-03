### 简单推导

#### Homography推导

给定平面$\Pi$，在相机1和相机2下都可以观测到平面上的一点$P$，已知:

1. 由相机1到相机2的三维坐标变换为$X_2=RX_1+T$
2. 平面$\Pi$的法向量$\vec{n}$在相机1下的参数表示为$n=(x,y,z)^T$

于是可以得到:
$$
X_2 = RX_1+ T \\
n^TX_1 = d \Rightarrow \frac{n^TX_1}{d} =1
$$
变形可以得到：
$$
X_2 = RX_1 + \frac{Tn^TX_1}{d}
$$


再将$x_1=KX_1,x_2 =KX_2$代入(此处省略了齐次坐标到非齐次坐标的变换)，可以得到：
$$
x_2 = K(R+ \frac{Tn^T}{d})K^{-1}x_1
$$

#### 本文中的Homography的推导

按照论文表述为从Reference Image到各个Source Image的变换

最终形式为：
$$
\mathbf{H}_{i}(d)=\mathbf{K}_{i} \cdot \mathbf{R}_{i} \cdot\left(\mathbf{I}-\frac{\left(\mathbf{t}_{1}-\mathbf{t}_{i}\right) \cdot \mathbf{n}_{1}^{T}}{d}\right) \cdot \mathbf{R}_{1}^{T} \cdot \mathbf{K}_{1}^{T}
$$
猜测$\mathbf{K}_1^T$这里应该是$\mathbf{K}_1^{-1}$．不然形式对不上

对式子进行变形，可以得到(以下省略黑体)：
$$
H_i(d) =K_i\cdot (R_iR_1^T + \frac{R_i(t_i-t_1) \cdot n_1^T \cdot R_1^T}{d}) \cdot K_1^{-1}\\
\Rightarrow H_i(d) =K_i\cdot (R_iR_1^T + \frac{R_i(t_i-t_1) \cdot (R_1 \cdot  n_1)^T }{d}) \cdot K_1^{-1}
$$
根据式中表达式可以推测出$R_i,t_i$为世界坐标系到相机坐标系的变换.

与式子$x_2 = K(R+ \frac{Tn^T}{d})K^{-1}x_1$进行对应．

设$T_i = \begin{bmatrix}R_i & t_i \\ 0 & 1 \end{bmatrix}$,$T_1 = \begin{bmatrix}R_1 & t_1 \\ 0 & 1 \end{bmatrix}$

则由1系到$i$系的坐标变换为:$T_{i1}=T_iT_1^{-1}= \begin{bmatrix}R_iR_1^T & -R_iR_1^Tt_1+ t_i  \\0 &1\end{bmatrix}$

对应式$x_2 = K(R+ \frac{Tn^T}{d})K^{-1}x_1$中的$R,T$
$$
H_i(d) = K_i\cdot[R_iR_1^T + \frac{(-R_iR_1^Tt_1+t_i)\cdot (N_1)^T}{d} ]\cdot K_1^{-1}
$$

如果平面法向量是在世界坐标系下，则需要将其变换到相机1坐标系下，为$N_1 =R_1n_1$

于是:
$$
H_i(d) = K_i\cdot[R_iR_1^T + \frac{(-R_iR_1^Tt_1+t_i)\cdot (R_1n_1)^T}{d} ]\cdot K_1^{-1}
$$
论文中为:
$$
H_i(d) =K_i\cdot (R_iR_1^T + \frac{R_i(t_i-t_1) \cdot (R_1 \cdot  n_1)^T }{d}) \cdot K_1^{-1}
$$

#### 结论

一切尽在代码中．．．．．．．．．．．．．．．．．．．．．