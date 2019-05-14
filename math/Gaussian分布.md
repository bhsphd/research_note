### 高斯分布

#### 单变量高斯分布

##### 基础性质

一维高斯分布概率密度函数为：
$$
\mathcal{N}(x | \mu, \sigma) \stackrel{\mathrm{def}}{=} \frac{1}{\sqrt{2 \pi \sigma^{2}}} e^{-\frac{1}{2 \sigma^{2}}(x-\mu)^{2}}
$$
$\mu$为均值，$\sigma$为标准差.
$$
\begin{aligned} \mu & \stackrel{\mathrm{def}}{=} \quad E X=\int_{-\infty}^{\infty} x p(x) d x \\ \sigma^{2} & \stackrel{\mathrm{def}}{=} \quad \operatorname{Var} X=E(X-\mu)^{2} \\ &=\int(x-\mu)^{2} p(x) d x \\ &=\int x^{2} p(x) d x+\mu^{2} \int p(x) d x-2 \mu \int x p(x) d x \\ &=E\left[X^{2}\right]-\mu^{2} \end{aligned}
$$
一个常用的结论是:$E[X^2]={\mu}^2 + {\sigma}^2$. $\frac{1}{\sqrt{2 \pi \sigma^{2}}}$为归一化常数．

+ pdf(Probability density function )  
+ cdf(Cumulative distribution function)定义为$\Phi(x)=\int_{-\infty}^{x} p(z) d z$ 

假设$X\sim \mathcal{N}(\mu,\sigma^2)$，则: $Z=(X-\mu)/\sigma \sim \mathcal{N}(0,1)$．$2\sigma$概率为：
$$
\begin{aligned} p(a<X<b) &=p\left(\frac{a-\mu}{\sigma}<Z<\frac{b-\mu}{\sigma}\right) \\ &=\Phi\left(\frac{b-\mu}{\sigma}\right)-\Phi\left(\frac{a-\mu}{\sigma}\right) \end{aligned}
$$

##### 最大似然估计

假设样本$X_i\sim \mathcal{N}(\mu,\sigma^2)$，
$$
\begin{aligned} p\left(\mathcal{D} | \mu, \sigma^{2}\right) &=\prod_{i=1}^{N} \mathcal{N}\left(x_{i} | \mu, \sigma^{2}\right) \\ \ell\left(\mu, \sigma^{2}\right) &=-\frac{1}{2 \sigma^{2}} \sum_{i=1}^{N}\left(x_{i}-\mu\right)^{2}-\frac{N}{2} \ln \sigma^{2}-\frac{N}{2} \ln (2 \pi) \end{aligned}
$$
求偏导可以得到：
$$
\begin{aligned} \frac{\partial L}{\partial \mu} &=-\frac{2}{2 \sigma^{2}} \sum_{i}\left(x_{i}-\mu\right)=0 \\ \mu_{M L} &=\frac{1}{N} \sum_{i=1}^{N} x_{i} \end{aligned}
$$
令$v=\sigma^2,\mu = \hat{\mu}_{ML} $ ，则：
$$
\begin{aligned} \frac{\partial L}{\partial v} &=\frac{1}{2} v^{-2} \sum_{i}\left(x_{i}-\mu\right)-\frac{N}{2 v}=0 \\ \hat{\sigma}^{2}_{M L} &=\frac{1}{N} \sum_{i=1}^{N}\left(x_{i}-\mu_{M L}\right)^{2} \\ &=\frac{1}{N}\left[\sum_{i} x_{i}^{2}+\sum_{i} \hat{\mu}^{2}-2 \sum_{i} x_{i} \hat{\mu}\right] \\ &=\frac{1}{N} \sum_{i}\left(x_{i}^{2}\right)-\left(\frac{1}{N} \sum_{i} x_{i}\right)^{2} \end{aligned}
$$

#### 多维高斯分布