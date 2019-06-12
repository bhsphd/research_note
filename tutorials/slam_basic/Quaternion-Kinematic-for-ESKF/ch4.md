[TOC]

### Perturbations, derivatives and integrals

#### 4.1 $SO(3)$上的加法和减法 

不同于普通的向量空间，在$SO(3)$流形上的普通加法和减法不再成立，因此必须重新定义加法和减法：$\oplus,\ominus$　

`加法`:
$$
\oplus : S O(3) \times \mathbb{R}^{3} \rightarrow S O(3)
$$
通常为$R$的切空间元素$\boldsymbol{\theta} \in \mathbb{R}^{3}$
$$
\mathrm{S}=\mathrm{R} \oplus \boldsymbol{\theta} \triangleq \mathrm{R} \circ \operatorname{Exp}(\boldsymbol{\theta}) \quad \mathrm{R}, \mathrm{S} \in S O(3), \boldsymbol{\theta} \in \mathbb{R}^{3}
$$

$$
\begin{aligned} \mathbf{q}_{\mathrm{S}} &=\mathbf{q}_{\mathrm{R}} \oplus \boldsymbol{\theta}=\mathbf{q}_{\mathrm{R}} \otimes \operatorname{Exp}(\boldsymbol{\theta}) \\ \mathbf{R}_{\mathrm{S}} &=\mathbf{R}_{\mathrm{R}} \oplus \boldsymbol{\theta}=\mathbf{R}_{\mathrm{R}} \cdot \operatorname{Exp}(\boldsymbol{\theta}) \end{aligned}
$$

`减法`:
$$
\ominus : S O(3) \times S O(3) \rightarrow \mathbb{R}^{3}
$$
为加法逆运算，通常用$R$的局部切空间元素表示，意为两个$SO(3)$元素之间的差异:
$$
\boldsymbol{\theta}=\mathrm{S} \ominus \mathrm{R} \triangleq \log \left(\mathrm{R}^{-1} \circ \mathrm{S}\right) \quad \mathrm{R}, \mathrm{S} \in S O(3), \boldsymbol{\theta} \in \mathbb{R}^{3}
$$

$$
\begin{aligned} \boldsymbol{\theta} &=\mathbf{q}_{\mathrm{S}} \ominus \mathbf{q}_{\mathrm{R}}=\log \left(\mathbf{q}_{\mathrm{R}}^{*} \otimes \mathbf{q}_{\mathrm{S}}\right) \\ \boldsymbol{\theta} &=\mathbf{R}_{\mathrm{S}} \ominus \mathbf{R}_{\mathrm{R}}=\log \left(\mathbf{R}_{\mathrm{R}}^{\top} \mathbf{R}_{\mathrm{S}}\right) \end{aligned}
$$

#### 4.2 导数的四种形式

##### 4.2.1 $f : \mathbb{R}^{m} \rightarrow \mathbb{R}^{n}$的导数

$$
\frac{\partial f(\mathbf{x})}{\partial \mathbf{x}} \triangleq \lim _{\delta \mathbf{x} \rightarrow 0} \frac{f(\mathbf{x}+\delta \mathbf{x})-f(\mathbf{x})}{\delta \mathbf{x}} \quad \in \mathbb{R}^{n \times m}
$$

$$
f(\mathbf{x}+\Delta \mathbf{x}) \approx f(\mathbf{x})+\frac{\partial f(\mathbf{x})}{\partial \mathbf{x}} \Delta \mathbf{x} \quad \in \mathbb{R}^{n}
$$

##### 4.2.2 $f : S O(3) \rightarrow S O(3)$的导数

$$
\begin{aligned} \frac{\partial f(\mathrm{R})}{\partial \boldsymbol{\theta}} & \triangleq \lim _{\delta \theta \rightarrow 0} \frac{f(\mathrm{R} \oplus \delta \boldsymbol{\theta}) \ominus f(\mathrm{R})}{\delta \boldsymbol{\theta}} \quad \in \mathbb{R^{3\times 3}} \\ &=\lim _{\delta \theta \rightarrow 0} \frac{\log \left(f^{-1}(\mathrm{R}) f(\mathrm{RExp}(\delta \boldsymbol{\theta}))\right)}{\delta \boldsymbol{\theta}} \end{aligned}
$$

$$
f(\mathrm{R} \oplus \Delta \boldsymbol{\theta}) \approx f(\mathrm{R}) \oplus \frac{\partial f(\mathrm{R})}{\partial \boldsymbol{\theta}} \Delta \theta \triangleq f(\mathrm{R}) \operatorname{Exp}\left(\frac{\partial f(\mathrm{R})}{\partial \boldsymbol{\theta}} \Delta \boldsymbol{\theta}\right) \quad \in S O(3)
$$

##### 4.2.3 $f : \mathbb{R}^{m} \rightarrow S O(3)$的导数

$$
\begin{aligned} \frac{\partial f(\mathbf{x})}{\partial \mathbf{x}} & \triangleq \lim _{\delta \mathbf{x} \rightarrow 0} \frac{f(\mathbf{x}+\delta \mathbf{x}) \ominus f(\mathbf{x})}{\delta \mathbf{x}} \quad \in \mathbb{R^{3\times m}}\\ &=\lim _{\delta \mathbf{x} \rightarrow 0} \frac{\log \left(f^{-1}(\mathbf{x}) f(\mathbf{x}+\delta \mathbf{x})\right)}{\delta \mathbf{x}} \end{aligned}
$$

$$
f(\mathbf{x}+\Delta \mathbf{x}) \approx f(\mathbf{x}) \oplus \frac{\partial f(\mathbf{x})}{\partial \mathbf{x}} \Delta \mathbf{x} \triangleq f(\mathbf{x}) \operatorname{Exp}\left(\frac{\partial f(\mathbf{x})}{\partial \mathbf{x}} \Delta \mathbf{x}\right) \quad \in S O(3)
$$

##### 4.2.4 $f : S O(3) \rightarrow \mathbb{R}^{n}$的导数 

$$
\begin{aligned} \frac{\partial f(\mathrm{R})}{\partial \boldsymbol{\theta}} & \triangleq \lim _{\delta \boldsymbol{\theta} \rightarrow 0} \frac{f(\mathrm{R} \oplus \delta \boldsymbol{\theta})-f(\mathrm{R})}{\delta \boldsymbol{\theta}} \quad \in \mathbb{R^{n\times 3}} \\ &=\lim _{\delta \theta \rightarrow 0} \frac{f(\operatorname{REp}(\delta \boldsymbol{\theta}))-f(\mathrm{R})}{\delta \boldsymbol{\theta}} \end{aligned}
$$

$$
f(\mathrm{R} \oplus \delta \boldsymbol{\theta}) \approx f(\mathrm{R})+\frac{\partial f(\mathrm{R})}{\partial \boldsymbol{\theta}} \Delta \boldsymbol{\theta} \triangleq f(\mathrm{R})+\operatorname{Exp}\left(\frac{\partial f(\mathrm{R})}{\partial \boldsymbol{\theta}} \Delta \boldsymbol{\theta}\right) \quad \in S O(3)
$$

#### 4.3 三维旋转中一些非常有用的$Jacobian$

考虑向量$\mathbf{a}$绕轴$\mathbf{u}$旋转角度$\theta$,

##### 4.3.1 旋转后的向量相对于原本向量的$Jacobian$

$$
\frac{\partial(\mathbf{q} \otimes \mathbf{a} \otimes \mathbf{q} *)}{\partial \mathbf{a}}=\frac{\partial(\mathbf{R} \mathbf{a})}{\partial \mathbf{a}}=\mathbf{R}
$$

##### 4.3.2 相对于四元数的$Jacobian$

$\mathbf{q}=[w \mathbf{v}]=w+\mathbf{v}$，利用性质：
$$
\mathbf{a} \times(\mathbf{b} \times \mathbf{c})=(\mathbf{c} \times \mathbf{b}) \times \mathbf{a}=\left(\mathbf{a}^{\top} \mathbf{c}\right) \mathbf{b}-\left(\mathbf{a}^{\top} \mathbf{b}\right) \mathbf{c}
$$
略，可以得到:
$$
\frac{\partial(\mathbf{q} \otimes \mathbf{a} \otimes \mathbf{q} *)}{\partial \mathbf{q}}=2\left[w \mathbf{a}+\mathbf{v} \times \mathbf{a} | \mathbf{v}^{\top} \mathbf{a} \mathbf{I}_{3}+\mathbf{v} \mathbf{a}^{\top}-\mathbf{a} \mathbf{v}^{\top}-w[\mathbf{a}]_{ \times}\right] \in \mathbb{R}^{3 \times 4}
$$


##### 4.3.3 $SO(3)$的右$Jacobian$

给定$R\in SO(3)$，对应的旋转向量为:$\boldsymbol{\theta} \in \mathbb{R}^{3}$,满足:$\mathrm{R}=\operatorname{Exp}(\boldsymbol{\theta})$．当对$\boldsymbol{\theta}$添加一个变换量$\delta \boldsymbol{\theta}$，对应的$SO(3)$元素$R$同样会发生变化，将变化量用$R$的切空间的一个旋转向量表示，为：$\delta \phi \in \mathbb{R}^{3}$
$$
\operatorname{Exp}(\boldsymbol{\theta}) \oplus \delta \boldsymbol{\phi}=\operatorname{Exp}(\boldsymbol{\theta}+\delta \boldsymbol{\theta})
$$
也可以写作:
$$
\operatorname{Exp}(\boldsymbol{\theta}) \circ \operatorname{Exp}(\delta \boldsymbol{\phi})=\operatorname{Exp}(\boldsymbol{\theta}+\delta \boldsymbol{\theta})
$$
于是:
$$
\delta \phi=\log \left(\operatorname{Exp}(\boldsymbol{\theta})^{-1} \circ \operatorname{Exp}(\boldsymbol{\theta}+\delta \boldsymbol{\theta})\right)=\operatorname{Exp}(\boldsymbol{\theta}+\delta \boldsymbol{\theta}) \ominus \operatorname{Exp}(\boldsymbol{\theta})
$$
利用极限，变化量$\delta \boldsymbol{\phi}$相对于$\delta \boldsymbol{\theta}$定义的$Jacobian$矩阵为：
$$
\frac{\partial \delta \phi}{\partial \delta \boldsymbol{\theta}}=\lim _{\delta \boldsymbol{\theta} \rightarrow 0} \frac{\delta \boldsymbol{\phi}}{\delta \boldsymbol{\theta}}=\lim _{\delta \boldsymbol{\theta} \rightarrow 0} \frac{\operatorname{Exp}(\boldsymbol{\theta}+\delta \boldsymbol{\theta}) \ominus \operatorname{Exp}(\boldsymbol{\theta})}{\delta \boldsymbol{\theta}}
$$
该导数$f : \mathbb{R}^{m} \rightarrow S O(3)$的一种特殊形式,函数为$f(\boldsymbol{\theta})=\operatorname{Exp}(\boldsymbol{\theta}), f:\mathbb{R}^3\rightarrow SO(3)$,该$Jacobian$矩阵通常被称作$SO(3)$的右$Jacobian$.
$$
\mathbf{J}_{r}(\boldsymbol{\theta}) \triangleq \frac{\partial \operatorname{Exp}(\boldsymbol{\theta})}{\partial \boldsymbol{\theta}}
$$
具体表达式根据使用的参数化方式不同而变化:
$$
\begin{aligned} \mathbf{J}_{r}(\boldsymbol{\theta}) &=\lim _{\delta \boldsymbol{\theta} \rightarrow 0} \frac{\operatorname{Exp}(\boldsymbol{\theta}+\delta \boldsymbol{\theta}) \ominus \operatorname{Exp}(\boldsymbol{\theta})}{\delta \boldsymbol{\theta}} \\ &=\lim _{\delta \theta \rightarrow 0} \frac{\log \left(\operatorname{Exp}(\boldsymbol{\theta})^{\top} \operatorname{Exp}(\boldsymbol{\theta}+\delta \boldsymbol{\theta})\right)}{\delta \boldsymbol{\theta}} \quad \text{if using} \quad \mathbf{R} \\ &=\lim _{\delta \boldsymbol{\theta} \rightarrow 0} \frac{\log \left(\operatorname{Exp}(\boldsymbol{\theta})^{*} \otimes \operatorname{Exp}(\boldsymbol{\theta}+\delta \boldsymbol{\theta})\right)}{\delta \boldsymbol{\theta}} \quad \text{if using} \quad \mathbf{q} \end{aligned}
$$
$SO(3)$的右$Jacobian$的解析形式为:
$$
\begin{aligned} \mathbf{J}_{r}(\boldsymbol{\theta}) &=\mathbf{I}-\frac{1-\cos \|\boldsymbol{\theta}\|}{\|\boldsymbol{\theta}\|^{2}}[\boldsymbol{\theta}]_{ \times}+\frac{\|\boldsymbol{\theta}\|-\sin \|\boldsymbol{\theta}\|}{\|\boldsymbol{\theta}\|^{3}}[\boldsymbol{\theta}]_{ \times}^{2} \\ \mathbf{J}_{r}^{-1}(\boldsymbol{\theta}) &=\mathbf{I}+\frac{1}{2}[\boldsymbol{\theta}]_{ \times}+\left(\frac{1}{\|\boldsymbol{\theta}\|^{2}}-\frac{1+\cos \|\boldsymbol{\theta}\|}{2\|\boldsymbol{\theta}\| \sin \|\boldsymbol{\theta}\|}\right)[\boldsymbol{\theta}]_{ \times}^{2} \end{aligned}
$$
$SO(3)$的右$Jacobian$性质如下(对于任意$\boldsymbol{\theta}$和小量$\delta\boldsymbol{\theta}$)
$$
\begin{aligned} \operatorname{Exp}(\boldsymbol{\theta}+\delta \boldsymbol{\theta}) & \approx \operatorname{Exp}(\boldsymbol{\theta}) \operatorname{Exp}\left(\mathbf{J}_{r}(\boldsymbol{\theta}) \delta \boldsymbol{\theta}\right) \\ \operatorname{Exp}(\boldsymbol{\theta}) \operatorname{Exp}(\delta \boldsymbol{\theta}) & \approx \operatorname{Exp}\left(\boldsymbol{\theta}+\mathbf{J}_{r}^{-1}(\boldsymbol{\theta}) \delta \boldsymbol{\theta}\right) \\ \log (\operatorname{Exp}(\boldsymbol{\theta}) \operatorname{Exp}(\delta \boldsymbol{\theta})) & \approx \boldsymbol{\theta}+\mathbf{J}_{r}^{-1}(\boldsymbol{\theta}) \delta \boldsymbol{\theta} \end{aligned}
$$
