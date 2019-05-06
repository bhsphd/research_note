## SLAM问题建模与非线性优化

### 状态估计问题

####  批量状态估计与最大后验估计

基本模型：运动模型和观测模型
$$
\left\{\begin{array}{l}{\boldsymbol{x}_{k}=f\left(\boldsymbol{x}_{k-1}, \boldsymbol{u}_{k}\right)+\boldsymbol{w}_{k}} \\ {\boldsymbol{z}_{k, j}=h\left(\boldsymbol{y}_{j}, \boldsymbol{x}_{k}\right)+\boldsymbol{v}_{k, j}}\end{array}\right.
$$
通常假设$\boldsymbol{w}_{k}, \boldsymbol{v}_{k, j}$  服从:
$$
\boldsymbol{w}_{k} \sim \mathcal{N}\left(\mathbf{0}, \boldsymbol{R}_{k}\right), \boldsymbol{v}_{k} \sim \mathcal{N}\left(\mathbf{0}, \boldsymbol{Q}_{k, j}\right)
$$
目标是通过带噪声的数据$\boldsymbol{z} $和$\boldsymbol{u}$推断出$\boldsymbol{x}$ 和$\boldsymbol{y}$ ，这就构成了状态估计问题。

 处理方式两种：

1. `incremental` or `filter-based`，估计当前时刻的状态，用新数据更新之
2. `Batch`方法，把0~k时刻数据集中在一起，同时估计。**当前主流方法**
3. 实用的手段是`Sliding Window Based`

##### 问题基本 定义

机器人状态和路标点信息：
$$
\boldsymbol{x}=\left\{\boldsymbol{x}_{1}, \ldots, \boldsymbol{x}_{N}\right\}, \quad \boldsymbol{y}=\left\{\boldsymbol{y}_{1}, \ldots, \boldsymbol{y}_{M}\right\}
$$
概率上描述就是已知$\boldsymbol{u}$和$\boldsymbol{z}$ 求状态$\boldsymbol{x,y}$ 的条件概率分布：
$$
P(\boldsymbol{x}, \boldsymbol{y} | \boldsymbol{z}, \boldsymbol{u})
$$
利用贝叶斯法则：
$$
P(\boldsymbol{x}, \boldsymbol{y} | \boldsymbol{z}, \boldsymbol{u})=\frac{P(\boldsymbol{z}, \boldsymbol{u} | \boldsymbol{x}, \boldsymbol{y}) P(\boldsymbol{x}, \boldsymbol{y})}{P(\boldsymbol{z}, \boldsymbol{u})}\propto \underbrace{P(\boldsymbol{z}, \boldsymbol{u} | \boldsymbol{x}, \boldsymbol{y}) }_{\text{似然}}\underbrace{P(\boldsymbol{x}, \boldsymbol{y})}_{ \text{先验}}
$$
左侧称为后验概率，右侧的$P(\boldsymbol{z} | \boldsymbol{x})$ 称为似然(Likehood)，$P(\boldsymbol{x})$ 为先验(Prior)，直接求后验分布很困难，但是求一个状态的最优估计，使得在该状态下后验概率最大化(Maximize a Posterior, **MAP**) 是可行的：
$$
(\boldsymbol{x}, \boldsymbol{y})^{*}_{\mathrm{MAP}}=\arg \max P(\boldsymbol{x}, \boldsymbol{y} | \boldsymbol{z}, \boldsymbol{u})=\arg \max P(\boldsymbol{z}, \boldsymbol{u} | \boldsymbol{x}, \boldsymbol{y}) P(\boldsymbol{x}, \boldsymbol{y})
$$
求解最大后验概率等价于最大化似然和先验的乘积，进一步可以不考虑先验，进而求解最大似然估计(Maximize Likehood Estimation,MLE):
$$
(\boldsymbol{x}, \boldsymbol{y})_{\mathrm{MLE}}^{*}=\arg \max P(\boldsymbol{z}, \boldsymbol{u} | \boldsymbol{x}, \boldsymbol{y})
$$
可以理解为："在什么样的状态下,最可能产生现在观测到的数据"

#### 最小二乘问题引出

对于某一次观测:
$$
z_{k, j}=h\left(\boldsymbol{y}_{j}, \boldsymbol{x}_{k}\right)+\boldsymbol{v}_{k, j}
$$
由于假设了噪声:$\boldsymbol{v}_{k} \sim \mathcal{N}\left(\mathbf{0}, \boldsymbol{Q}_{k, j}\right)$，所以观测数据的条件概率为：
$$
P\left(\boldsymbol{z}_{j, k} | \boldsymbol{x}_{k}, \boldsymbol{y}_{j}\right)=N\left(h\left(\boldsymbol{y}_{j}, \boldsymbol{x}_{k}\right), \boldsymbol{Q}_{k, j}\right)
$$
依然是一个高斯分布，利用最小化负对数来求高斯分布的最大似然。

考虑$\boldsymbol{x} \sim \mathcal{N}(\boldsymbol{\mu}, \mathbf{\Sigma})$ ，概率密度展开形式为：
$$
P(\boldsymbol{x})=\frac{1}{\sqrt{(2 \pi)^{N} \operatorname{det}(\boldsymbol{\Sigma})}} \exp \left(-\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu})^{\mathrm{T}} \boldsymbol{\Sigma}^{-1}(\boldsymbol{x}-\boldsymbol{\mu})\right)
$$
取负对数，求最大值:
$$
-\ln (P(\boldsymbol{x}))=\frac{1}{2} \ln \left((2 \pi)^{N} \operatorname{det}(\boldsymbol{\Sigma})\right)+\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu})^{\mathrm{T}} \boldsymbol{\Sigma}^{-1}(\boldsymbol{x}-\boldsymbol{\mu})
$$
等价于：
$$
\begin{aligned}\left(\boldsymbol{x}_{k}, \boldsymbol{y}_{j}\right)^{*} &=\arg \max \mathcal{N}\left(h\left(\boldsymbol{y}_{j}, \boldsymbol{x}_{k}\right), \boldsymbol{Q}_{k, j}\right) \\ &=\arg \min \left(\left(\boldsymbol{z}_{k, j}-h\left(\boldsymbol{x}_{k}, \boldsymbol{y}_{j}\right)\right)^{\mathrm{T}} \boldsymbol{Q}_{k, j}^{-1}\left(\boldsymbol{z}_{\boldsymbol{k}, \boldsymbol{j}}-h\left(\boldsymbol{x}_{k}, \boldsymbol{y}_{j}\right)\right)\right) \end{aligned}
$$
我们发现，该式等价于最小化噪声(误差) 的一个二次型，称为：Mahalanobis distance又称为**马氏距离** 。可以看成由$Q_{k, j}^{-1}$ 加权之后的欧氏距离(二范数)，这里的$Q_{k, j}^{-1}$称为信息矩阵，即高斯分布协方差矩阵的逆。考虑批量时刻的数据，通常假设各个时刻输入和观测是相互独立的。得到联合分布：
$$
P(\boldsymbol{z}, \boldsymbol{u} | \boldsymbol{x}, \boldsymbol{y})=\prod_{k} P\left(\boldsymbol{u}_{k} | \boldsymbol{x}_{k-1}, \boldsymbol{x}_{k}\right) \prod_{k, j} P\left(\boldsymbol{z}_{k, j} | \boldsymbol{x}_{k}, \boldsymbol{y}_{j}\right)
$$
定义各次输入和观测数据与模型之间的误差：
$$
\begin{array}{l}{\boldsymbol{e}_{u, k}=\boldsymbol{x}_{k}-f\left(\boldsymbol{x}_{k-1}, \boldsymbol{u}_{k}\right)} \\ {\boldsymbol{e}_{z, j, k}=\boldsymbol{z}_{k, j}-h\left(\boldsymbol{x}_{k}, \boldsymbol{y}_{j}\right)}\end{array}
$$
那么最小化所有时刻估计值与真实读数之间的马氏距离等价于求最大似然估计，即：
$$
\min J(\boldsymbol{x}, \boldsymbol{y})=\sum_{k} \boldsymbol{e}_{u, k}^{\mathrm{T}} \boldsymbol{R}_{k}^{-1} \boldsymbol{e}_{u, k}+\sum_{k} \sum_{j} e_{z, k, j}^{\mathrm{T}} \boldsymbol{Q}_{k, j}^{-1} \boldsymbol{e}_{z, k, j}
$$
这样就得到了一个最小二乘问题(Least Square Problem)，它的解等价于状态的最大似然估计。上式有如下特点：

+ 问题的目标函数由许多个误差(加权)构成，虽然总体状态维数高，但是每一个误差都比较简单，仅与一两个状态变量有关。整个问题有一种稀疏的形式。
+ 使用李代数表示增量，问题转化为一个无约束最小二乘问题。
+ 使用二次型度量误差，使得不同观测权重不同。

#### 批量状态估计的例子

考虑一个简单的离散时间系统：
$$
\begin{aligned}
x_{k}=&x_{k-1}+u_{k}+w_{k}, \quad {w_{k} \sim \mathcal{N}\left(0, Q_{k}\right)} \\ 
z_{k}=&x_{k}+n_{k}, \quad n_{k} \sim \mathcal{N}\left(0, R_{k}\right)
\end{aligned}
$$
取时间$k=1,\cdots ,3$ ，设初始状态$x_0$已知，下面推导batch状态的最大似然估计：
令状态变量为：$\boldsymbol{x}=\left[x_{0}, x_{1}, x_{2}, x_{3}\right]^{\mathrm{T}}$，观测为：$\boldsymbol{z}=\left[z_{1}, z_{2}, z_{3}\right]^{\mathrm{T}}$，输入$\boldsymbol{u}=\left[u_{1}, u_{2}, u_{3}\right]^{\mathrm{T}}$，最大似然估计为：
$$
\begin{aligned} x_{\mathrm{map}}^{*} &=\arg \max P(\boldsymbol{x} | \boldsymbol{u}, \boldsymbol{z})=\arg \max P(\boldsymbol{u}, \boldsymbol{z} | \boldsymbol{x}) \\ &=\prod_{k=1}^{3} P\left(u_{k} | x_{k-1}, x_{k}\right) \prod_{k=1}^{3} P\left(z_{k} | x_{k}\right) \end{aligned}
$$

$$
P\left(u_{k} | x_{k-1}, x_{k}\right)=\mathcal{N}\left(x_{k}-x_{k-1}, Q_{k}\right)
$$

$$
P\left(z_{k} | x_{k}\right)=\mathcal{N}\left(x_{k}, R_{k}\right)
$$

构建误差变量：
$$
e_{u, k}=x_{k}-x_{k-1}-u_{k}, \quad e_{z, k}=z_{k}-x_{k}
$$
最小二乘目标函数为：
$$
\min \sum_{k=1}^{3} e_{u, k}^{\mathrm{T}} Q_{k}^{-1} e_{u, k}+\sum_{k=1}^{3} e_{z, k}^{\mathrm{T}} R_{k}^{-1} e_{z, k}
$$
此系统为线性系统，可以写成向量形式：定义$\boldsymbol{y}=[\boldsymbol{u}, \boldsymbol{z}]^{\mathrm{T}}$，
$$
y-H x=e \sim \mathcal{N}(0, \Sigma)
$$
其中:
$$
H=\left[ \begin{array}{cccc}{1} & {-1} & {0} & {0} \\ {0} & {1} & {-1} & {0} \\ {0} & {0} & {1} & {-1} \\ \hline 0 & {1} & {0} & {0} \\ {0} & {0} & {1} & {0} \\ {0} & {0} & {0} & {1}\end{array}\right]
$$
且$\boldsymbol{\Sigma}=\operatorname{diag}\left(Q_{1}, Q_{2}, Q_{3}, R_{1}, R_{2}, R_{3}\right)$，整个问题可以写成：
$$
\boldsymbol{x}_{\mathrm{map}}^{*}=\arg \min e^{\mathrm{T}} \boldsymbol{\Sigma}^{-1} \boldsymbol{e}
$$
问题的唯一解：
$$
\boldsymbol{x}_{\mathrm{map}}^{*}=\left(\boldsymbol{H}^{\mathrm{T}} \boldsymbol{\Sigma}^{-1} \boldsymbol{H}\right)^{-1} \boldsymbol{H}^{\mathrm{T}} \boldsymbol{\Sigma}^{-1} \boldsymbol{y}
$$
