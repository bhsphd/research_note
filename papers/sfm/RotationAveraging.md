[TOC]

### Rotation Averaging

#### 一、背景知识

三维旋转可以用旋转矩阵表示：$\mathbf{R}\in\mathbf{SO(3)}$，可以通过李代数的方式将其简化：
$$
\mathbf{R}(\omega)=e^{[\boldsymbol{\omega}]\times}
$$
由于李群不具有交换性，所以$e^{\mathbf{x}} e^{\mathbf{y}}=e^{\mathbf{x}+\mathbf{y}}$并不成立，因此需要利用李括号和BCH公式来进行表示

`李括号`
$$
[\mathbf{x}, \mathbf{y}]=\mathbf{x} \mathbf{y}-\mathbf{y} \mathbf{x}
$$

$$
\begin{array}{c}{[\mathbf{x}, \mathbf{y}]=-[\mathbf{y}, \mathbf{x}](anti- symmetry) } \\ {[\mathbf{x},[\mathbf{y}, \mathbf{z}]]+[\mathbf{y},[\mathbf{z}, \mathbf{x}]]+[\mathbf{z},[\mathbf{x}, \mathbf{y}]]=0( Jacobi\quad identity )}\end{array}
$$



`BCH公式`
$$
B C H(\mathbf{x}, \mathbf{y})=\mathbf{x}+\mathbf{y}+\frac{1}{2}[\mathbf{x}, \mathbf{y}]+\frac{1}{12}[\mathbf{x}-\mathbf{y},[\mathbf{x}, \mathbf{y}]]+\mathcal{O}\left(|(\mathbf{x}, \mathbf{y})|^{4}\right)
$$

于是:
$$
e^{\mathbf{x}} e^{\mathbf{y}}=e^{B C H(\mathbf{x}, \mathbf{y})}
$$

利用李代数可以定义两个旋转矩阵之间的距离为：
$$
d\left(\mathbf{R}_{1}, \mathbf{R}_{2}\right)=\frac{1}{\sqrt{2}}\left\|\log \left(\mathbf{R}_{1} \mathbf{R}_{2}^{-1}\right)\right\|_{F}=\frac{1}{\sqrt{2}}\left\|\log \left(\mathbf{R}_{2} \mathbf{R}_{1}^{-1}\right)\right\|_{F}
$$

一般称为`intrinsic bi-invariant distance on SO(3)`



#### 二、Rotation Averaging

利用上述定义的距离度量公式，可以得到一系列旋转之间的平均值(intrinsic average)
$$
\left\{\mathbf{R}_{1}, \cdots, \mathbf{R}_{n}\right\}, \mu \in SO(3) \\
\arg \min _{\mu \in S O(3)} \sum_{k=1}^{n} d^{2}\left(\mathbf{R}_{k}, \mu\right) 
$$
这个优化问题没有封闭解，只能通过迭代的方式去求解，对目标函数进行修改可以得到RotationAveraging问题的优化目标函数：
$$
\mathbf{R}_{g l o b a l}=\left\{\mathbf{R}_{1}, \cdots, \mathbf{R}_{N}\right\} \\
\arg \min _{\mathbf{R}_{g l o b a l}} \min _{\mathbf{R}_{g l o b a l}} \sum_{(i, j) \in \mathcal{E}} d^{2}\left(\mathbf{R}_{i j}, \mathbf{R}_{j} \mathbf{R}_{i}^{-1}\right)
$$

对与两视图之间的旋转关系：$\mathbf{R}_{i j}=\mathbf{R}_{j} \mathbf{R}_{i}^{-1}$，利用BCH公式将其写成李代数的形式

可以表示成：$\boldsymbol{\omega}_{i j}=\boldsymbol{\omega}_{j}-\boldsymbol{\omega}_{i}$(此处取了一阶近似)
$$
\omega_{i j}=\omega_{j}-\omega_{i}=\underbrace{[\cdots-\mathbf{I} \cdots \mathbf{I} \cdots]}_{\mathbf{A}_{i j}} \boldsymbol{\omega}_{g l o b a l}
$$
其中:$\omega_{g l o b a l}=\left[\omega_{1}, \cdots, \omega_{N}\right]^{T}$

整个式子可以写成：
$$
\mathbf{A} \omega_{g l o b a l}=\boldsymbol{\omega}_{r e l}
$$

求解方法可以按照算法一进行：
`算法一`

+ 输入:$\left\{\mathbf{R}_{i j 1}, \cdots, \mathbf{R}_{i j k}\right\}$相对旋转
+ 输出:$\mathbf{R}_{g l o b a l}=\left\{\mathbf{R}_{1}, \cdots, \mathbf{R}_{N}\right\}$全局的绝对姿态
+ 初始：给定$\mathbf{R}_{g l o b a l}$一个初始估计
  1. $\Delta \mathbf{R}_{i j}=\mathbf{R}_{j}^{-1} \mathbf{R}_{i j} \mathbf{R}_{i}$
  2. $\Delta \boldsymbol{\omega}_{i j}=\log \left(\Delta \mathbf{R}_{i j}\right)$
  3. 求解$\mathbf{A} \Delta \boldsymbol{\omega}_{g l o b a l}=\Delta \boldsymbol{\omega}_{r e l}$
  4. $\forall k \in[1, N], \mathbf{R}_{k}=\mathbf{R}_{k} \exp \left(\Delta \boldsymbol{\omega}_{k}\right)$
  5. 循环迭代，直到$\Vert\Delta \boldsymbol{\omega}_{i j} \Vert < \epsilon$为止。



#### 三、$l_1$范数Rotation Averaging

利用$l_2$范数对上述算法中的3进行求解容易因为outliers的影响导致结果错误，可以考虑$l_1$范数优化的方式对其进行求解。

对于$\mathbf{A} \mathbf{x}=\mathbf{b}$的求解，在存在outliers的情况下等式可以重写为：$\mathbf{b}=\mathbf{A} \mathbf{x}+\mathbf{e}$

根据陶轩哲的论文`Decoding by linear programming`已经证明了，可以通过求解：
$$
\arg \min _{\mathbf{x}}\|\mathbf{A} \mathbf{x}-\mathbf{b}\|_{\ell_{1}}
$$
高效而又精确地得到$\mathbf{x}$。在本文中对应的求解方程为：
$$
\Delta \omega_{r e l}=\mathbf{A} \Delta \omega_{g l o b a l}+\mathbf{e}
$$
对应算法一中的第三步的求解：$\left\|\mathbf{A} \Delta \boldsymbol{\omega}_{g l o b a l}-\Delta \boldsymbol{\omega}_{r e l}\right\|_{\ell_{1}}$

由于矩阵$\mathbf{A}$的每一行只有两个非零元素且为$\{-1,+1\}$，所以矩阵$\mathbf{A}$有较好地稀疏性。



#### 四、IRLS Rotation Averaging

$l_1$范数优化提供了较好地应对outliers的措施，利用$l_2$范数优化可以得到更加精确的结果，并且利用$l_1$范数给出的结果作为初值，$l_2$范数优化受到outliers的影响较小。

利用Iteratively Reweighted Least Squares (IRLS)求解

`算法二` IRLS

+ 初始：设$\mathbf{x}$为初始值
  1. $\mathbf{x}_{\text {prev}} \leftarrow \mathbf{x}$
  2. $\mathbf{e} \leftarrow \mathbf{A} \mathbf{x}-\mathbf{b}$
  3. $\Phi \leftarrow \Phi(\mathrm{e})$
  4. $\mathbf{x} \leftarrow\left(\mathbf{A}^{T} \mathbf{\Phi} \mathbf{A}\right)^{-1} \mathbf{A} \mathbf{\Phi} \mathbf{b}$
  5. 循环迭代，直到$\Vert\Delta \boldsymbol{\omega}_{i j} \Vert < \epsilon$为止。

利用鲁棒cost function:
$$
E=\sum_{i} \rho\left(\left\|\mathbf{e}_{i}\right\|\right)
$$
其中: $\mathbf{e}=\mathbf{A} \mathbf{x}-\mathbf{b}$

loss function选择为Huber-liked: $\rho(x)=\frac{x^{2}}{x^{2}+\sigma^{2}}$
$$
\begin{aligned} \min _{\mathbf{x}} E=\min _{\mathbf{x}} \sum_{i} \rho\left(\left\|\mathbf{e}_{i}\right\|\right) &=\min _{\mathbf{x}} \sum_{i} \frac{\mathbf{e}_{i}^{2}}{\mathbf{e}_{i}^{2}+\sigma^{2}} \\ \Rightarrow \frac{\partial E}{\partial \mathbf{x}} &=\frac{\partial E}{\partial \mathbf{e}} \frac{\partial \mathbf{e}}{\partial \mathbf{x}}=0 \\ \Rightarrow \mathbf{A}^{T} \mathbf{\Phi}(\mathbf{e}) \mathbf{A} \mathbf{x} &=\mathbf{A}^{T} \mathbf{\Phi}(\mathbf{e}) \mathbf{b} \end{aligned}
$$
其中$\Phi(\mathbf{e})$为对角阵，其对角元素为：$\Phi(i, i)=\frac{\sigma^{2}}{\left(\mathbf{e}_{i}^{2}+\sigma^{2}\right)^{2}}$

由于$\Phi(\mathbf{e})$与$\mathbf{x}$也有关，于是上式常用迭代的方式求解：
首先保持$\mathbf{x}$固定，计算$\mathbf{e}=\mathbf{A} \mathbf{x}-\mathbf{b}$，接着固定$\Phi$，可以假设$\Phi$与$\mathbf{x}$无关。
$$
\min _{\mathbf{x}}(\mathbf{A} \mathbf{x}-\mathbf{b})^{T} \boldsymbol{\Phi}(\mathbf{A} \mathbf{x}-\mathbf{b})
$$
最优解为:$\left(\mathbf{A}^{T} \boldsymbol{\Phi} \mathbf{A}\right)^{-1} \mathbf{A} \boldsymbol{\Phi} \mathbf{b}$，利用计算出的$\mathbf{x}$来更新$\Phi$，再重新计算$\mathbf{x}$不断进行交替迭代。



#### 五、整体算法流程

+ L1RA:
  + 初始化$\mathbf{R}_{g l o b a l}$
  + 执行算法一，第3步使用$l_1$优化求解
+ IRLS:
  + 将L1RA的结果设为输入
  + 执行算法一，其中第3步使用算法2来完成

通常情况$l_1$优化只需要执行几次就行，不需要收敛，再利用$l_2$优化进行快速的地迭代即可。



#### 五、参考资料

+ `ICCV2013`:Efficient and Robust Large-Scale Rotation Averaging
+ `Tao`: Decoding by linear programming
+ `L1-magic`:<http://statweb.stanford.edu/~candes/l1magic/>

