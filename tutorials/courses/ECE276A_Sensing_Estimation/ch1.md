### Linear Algebra and Probability Theory(Review) 

#### 线性代数复习

##### 舒尔补

+ 定义 [wiki](../../../math/schur_complement.md)
+ 实对称矩阵A，矩阵$M=\left[ \begin{array}{ll}{A} & {B} \\ {B^{T}} & {D}\end{array}\right]$半正定条件：A与$S_A$都半正定或D与$S_D$都是半正定
+ $\frac{1}{2} x^{T} A x+b^{T} x+c=\frac{1}{2}\left(x+A^{-1} b\right)^{T} A\left(x+A^{-1} b\right)+c-\frac{1}{2} b^{T} A^{-1} b$

#### 矩阵求逆法则

+ Woodbury: $(A+B D C)^{-1}=A^{-1}-A^{-1} B\left(C A^{-1} B+D^{-1}\right)^{-1} C A^{-1}$
+ $\left[ \begin{array}{ll}{A} & {B} \\ {C} & {D}\end{array}\right]^{-1}==\left[ \begin{array}{cc}{\left(A-B D^{-1} C\right)^{-1}} & {-\left(A-B D^{-1} C\right)^{-1} B D^{-1}} \\ {-D^{-1} C\left(A-B D^{-1} C\right)^{-1}} & {D^{-1}+D^{-1} C\left(A-B D^{-1} C\right)^{-1} B D^{-1}}\end{array}\right]$

##### 求导法则(分子布局)

$$
\frac{d \mathbf{y}}{d x}=\left[ \begin{array}{c}{\frac{d \mathbf{y}_{1}}{d x}} \\ {\vdots} \\ {\frac{d \mathbf{y}_{m}}{d x}}\end{array}\right] \quad \frac{d \mathbf{Y}}{d x}=\left[ \begin{array}{ccc}{\frac{d \mathbf{Y}_{11}}{d x}} & {\cdots} & {\frac{d \mathbf{Y}_{1 n}}{d x}} \\ {\vdots} & {\ddots} & {\vdots} \\ {\frac{d \mathbf{Y}_{m 1}}{d x}} & {\cdots} & {\frac{d \mathbf{Y}_{m n}}{d x}}\end{array}\right] \\
\frac{d y}{d \mathbf{x}}=\underbrace{\left[ \begin{array}{ccc}{\frac{d y}{d x_{1}}} & {\cdots} & {\frac{d y}{d x_{p}}}\end{array}\right]}_{\left[\nabla_{x} y\right]^{T}(\text { gradient transpose })} \\
\frac{d \mathbf{y}}{d \mathbf{x}}=\underbrace{\left[ \begin{array}{ccc}{\frac{d \mathbf{y}_{1}}{d \mathbf{x}_{1}}} & {\cdots} & {\frac{d \mathbf{y}_{1}}{d \mathbf{x}_{p}}} \\ {\vdots} & {\ddots} & {\vdots} \\ {\frac{d \mathbf{y}_{m}}{d \mathbf{x}_{1}}} & {\cdots} & {\frac{d \mathbf{y}_{m}}{d \mathbf{x}_{p}}}\end{array}\right]}_{\text { Jacobian }} \frac{d \mathbf{Y}}{d \mathbf{x}} \in \mathbb{R}^{m \times n \times p} \\
$$

#### 概率论复习

##### 数学期望

$$
\mathbb{E}[g(X)]=\int g(x) p(x) d x
$$

##### 方差

$$
\begin{aligned} \operatorname{Var}[g(X)] &=\mathbb{E}\left[(g(X)-\mathbb{E}[g(X)])(g(X)-\mathbb{E}[g(X)])^{T}\right] \\ &=\mathbb{E}\left[g(X) g(X)^{T}\right]-\mathbb{E}[g(X)] \mathbb{E}[g(X)]^{T} \end{aligned}
$$

$$
\begin{aligned} \operatorname{Var}\left(\sum_{i=1}^{n} X_{i}\right) &=\sum_{i=1}^{n} \operatorname{Var}\left(X_{i}\right)+\sum_{i=1}^{n} \sum_{j \neq i} \operatorname{Cov}\left(X_{i}, X_{j}\right) \\ \operatorname{Cov}\left(X_{i}, X_{j}\right) &=\mathbb{E}\left(\left(X_{i}-\mathbb{E} X_{i}\right)\left(X_{j}-\mathbb{E} X_{j}\right)^{T}\right)=\mathbb{E}\left(X_{i} X_{j}^{T}\right)-\mathbb{E} X_{i} \mathbb{E} X_{j}^{T} \end{aligned}
$$

##### 贝叶斯公式

$$
p(x, y)=p(y | x) p(x)=p(x | y) p(y) \Rightarrow p(x | y)=\frac{p(y | x) p(x)}{\int p\left(y | x^{\prime}\right) p\left(x^{\prime}\right) d x^{\prime}}
$$

##### 高斯分布

`马氏距离`
$$
\|x\|_{S}^{2} :=x^{T} S^{-1} x
$$
`高斯随机变量`$X \sim \mathcal{N}(\mu, \Sigma)$

+ 参数： 均值$\mu \in \mathbb{R}^{n}$，协方差$\Sigma \in \mathbb{S}_{>0}^{n}$
+ 概率密度函数：$\phi(x ; \mu, \Sigma) :=\frac{1}{\sqrt{(2 \pi)^{n} \operatorname{det}(\Sigma)}} \exp \left(-\frac{1}{2}(x-\mu)^{-T} \Sigma^{-1}(x-\mu)\right)$
+ 期望：$\mathbb{E}[X]=\int x \phi(x ; \mu, \Sigma) d x=\mu$
+ 方差： $\operatorname{Var}[X]=\Sigma$

`高斯混合模型`$X \sim \mathcal{N} \mathcal{M}\left(\left\{\alpha_{k}\right\},\left\{\mu_{k}\right\},\left\{\Sigma_{k}\right\}\right)$

+ 参数： 权重$\alpha_{k} \geq 0, \sum_{k} \alpha_{k}=1$，均值$\mu_{k} \in \mathbb{R}^{n}$，协方差：$\Sigma_{k} \in \mathbb{S}_{\succeq 0}^{n}$
+ 概率密度函数：$p(x) :=\sum_{k} \alpha_{k} \phi\left(x ; \mu_{k}, \Sigma_{k}\right)$
+ 期望：$\mathbb{E}[X]=\int x p(x) d x=\sum_{k} \alpha_{k} \mu_{k}=: \overline{\mu}$
+ 方差：$\mathbb{E}\left[X X^{T}\right]-\mathbb{E}[X] \mathbb{E}[X]^{T}=\sum_{k} \alpha_{k}\left(\Sigma_{k}+\mu_{k} \mu_{k}^{T}\right)-\overline{\mu} \overline{\mu}^{T}$