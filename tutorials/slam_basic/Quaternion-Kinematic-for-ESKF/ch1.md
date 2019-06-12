[TOC]

#### Quaternion definition and properties 

##### 1.1 定义

###### 1.1.1 基本定义

给定两个复数$A=a+bi$ 和$C = c+di$,于是可以构造出四元数：$Q=A+Cj$,定义$k \triangleq ij$,于是可以得到四元数集合：$\mathbb{H}$ 
$$
Q=a+b i+c j+d k \in \mathbb{H}\\
i^2=j^2=k^2 =  -1 \\
ij=-ji=k,\quad jk=-kj=i,\quad ki=-ik=j
$$

> 注: JPL四元数定义中, $k=ji=-ij$

###### 1.1.2 表示方法

四元数可以写成如下形式：
$$
Q = q_w + q_xi+q_yj+q_zk \quad \Leftrightarrow \quad Q=q_w+ \mathbf{q}_v 
$$
其中$\mathbf{q}_v=q_xi+q_yj+q_zk=(q_x,q_y,q_z)$称为四元数的虚部.四元数也可以写成:
$$
Q=\left\langle q_{w}, \mathbf{q}_{v}\right\rangle
$$
不过平常使用的时候更多地会将四元数写成四维向量$\mathbf{q}$:
$$
\mathbf{q} \triangleq\left[\begin{array}{c}{q_{w}} \\ {\mathbf{q}_{v}}\end{array}\right]=\left[\begin{array}{c}{q_{w}} \\ {q_{x}} \\ {q_{y}} \\ {q_{z}}\end{array}\right]
$$
`general`: $\mathrm{q}=q_{w}+\mathbf{q}_{v}=\left[\begin{array}{l}{q_{w}} \\ {\mathbf{q}_{v}}\end{array}\right] \in \mathbb{H}$

`real`: $q_{w}=\left[\begin{array}{l}{q_{w}} \\ {\mathbf{0}_{v}}\end{array}\right] \in \mathbb{R}$

`pure`: $\mathbf{q}_{v}=\left[\begin{array}{l}{0} \\ {\mathbf{q}_{v}}\end{array}\right] \in \mathbb{H}_{p}$

##### 1.2 主要性质

###### 1.2.1 基本运算

`加法`:
$$
\mathbf{p} \pm \mathbf{q}=\left[\begin{array}{c}{p_{w}} \\ {\mathbf{p}_{v}}\end{array}\right] \pm\left[\begin{array}{c}{q_{w}} \\ {\mathbf{q}_{v}}\end{array}\right]=\left[\begin{array}{c}{p_{w} \pm q_{w}} \\ {\mathbf{p}_{v} \pm \mathbf{q}_{v}}\end{array}\right]
$$
`乘法`:
$$
\mathbf{p} \otimes \mathbf{q}=\left[\begin{array}{c}{p_{w} q_{w}-p_{x} q_{x}-p_{y} q_{y}-p_{z} q_{z}} \\ {p_{w} q_{x}+p_{x} q_{w}+p_{y} q_{z}-p_{z} q_{y}} \\ {p_{w} q_{y}-p_{x} q_{z}+p_{y} q_{w}+p_{z} q_{x}} \\ {p_{w} q_{z}+p_{x} q_{y}-p_{y} q_{x}+p_{z} q_{w}}\end{array}\right]
$$
写成向量形式为：
$$
\mathbf{p} \otimes \mathbf{q}=\left[\begin{array}{c}{p_{w} q_{w}-\mathbf{p}_{v}^{\top} \mathbf{q}_{v}} \\ {p_{w} \mathbf{q}_{v}+q_{w} \mathbf{p}_{v}+\mathbf{p}_{v} \times \mathbf{q}_{v}}\end{array}\right]
$$

> 注：四元数乘法不满足交换律$\mathbf{p} \otimes \mathbf{q} \neq \mathbf{q} \otimes \mathbf{p}$,但结合律和分配律成立.

###### 1.2.2 线性变换

由上方四元数的乘法可以看出，其为一个线性变换，可以写成矩阵形式：
$$
\mathbf{q}_{1} \otimes \mathbf{q}_{2}=\left[\mathbf{q}_{1}\right]_{L} \mathbf{q}_{2} \quad \mathbf{q}_{1} \otimes \mathbf{q}_{2}=\left[\mathbf{q}_{2}\right]_{R} \mathbf{q}_{1}
$$
其中$[\mathbf{q}]_{L}$和$[\mathbf{q}]_{R}$形式如下：
$$
[\mathbf{q}]_{L}=\left[\begin{array}{cccc}{q_{w}} & {-q_{x}} & {-q_{y}} & {-q_{z}} \\ {q_{x}} & {q_{w}} & {-q_{z}} & {q_{y}} \\ {q_{y}} & {q_{z}} & {q_{w}} & {-q_{x}} \\ {q_{z}} & {-q_{y}} & {q_{x}} & {q_{w}}\end{array}\right], \quad[\mathbf{q}]_{R}=\left[\begin{array}{ccc}{q_{w}} & {-q_{x}} & {-q_{y}} & {-q_{z}} \\ {q_{x}} & {q_{w}} & {q_{z}} & {-q_{y}} \\ {q_{y}} & {-q_{z}} & {q_{w}} & {q_{x}} \\ {q_{z}} & {q_{y}} & {-q_{x}} & {q_{w}}\end{array}\right]
$$


或者写成:
$$
[\mathbf{q}]_{L}=q_{w} \mathbf{I}+\left[\begin{array}{cc}{0} & {-\mathbf{q}_{v}^{\top}} \\ {\mathbf{q}_{v}} & {\left[\mathbf{q}_{v}\right]_{ \times}}\end{array}\right], \quad[\mathbf{q}]_{R}=q_{w} \mathbf{I}+\left[\begin{array}{cc}{0} & {-\mathbf{q}_{v}^{\top}} \\ {\mathbf{q}_{v}} & {-\left[\mathbf{q}_{v}\right]_{ \times}}\end{array}\right]
$$


其中:
$$
[\mathbf{a}]_{ \times} \triangleq\left[\begin{array}{ccc}{0} & {-a_{z}} & {a_{y}} \\ {a_{z}} & {0} & {-a_{x}} \\ {-a_{y}} & {a_{x}} & {0}\end{array}\right]
$$

$$
(\mathbf{q} \otimes \mathbf{x}) \otimes \mathbf{p}=[\mathbf{p}]_{R}[\mathbf{q}]_{L} \mathbf{x} \quad \quad \mathbf{q} \otimes(\mathbf{x} \otimes \mathbf{p})=[\mathbf{q}]_{L}[\mathbf{p}]_{R} \mathbf{x}
$$

可以得到如下性质:
$$
[\mathbf{p}]_{R}[\mathbf{q}]_{L}=[\mathbf{q}]_{L}[\mathbf{p}]_{R}
$$

###### 1.2.3 共轭 模 逆

`共轭`: $\mathbf{q}^{*} \triangleq q_{w}-\mathbf{q}_{v}=\left[\begin{array}{c}{q_{w}} \\ {-\mathbf{q}_{v}}\end{array}\right]$且 $\mathbf{q} \otimes \mathbf{q}^{*}=\mathbf{q}^{*} \otimes \mathbf{q}=q_{w}^{2}+q_{x}^{2}+q_{y}^{2}+q_{z}^{2}=\left[\begin{array}{c}{q_{w}^{2}+q_{x}^{2}+q_{y}^{2}+q_{z}^{2}} \\ {\mathbf{0}_{v}}\end{array}\right]$

$(\mathbf{p} \otimes \mathbf{q})^{*}=\mathbf{q}^{*} \otimes \mathbf{p}^{*}$

`模`: $\|\mathbf{q}\| \triangleq \sqrt{\mathbf{q} \otimes \mathbf{q}^{*}}=\sqrt{\mathbf{q}^{*} \otimes \mathbf{q}}=\sqrt{q_{w}^{2}+q_{x}^{2}+q_{y}^{2}+q_{z}^{2}} \in \mathbb{R}$ 且 $\|\mathbf{p} \otimes \mathbf{q}\|=\|\mathbf{q} \otimes \mathbf{p}\|=\|\mathbf{p}\|\|\mathbf{q}\|$

`逆`: $\mathbf{q} \otimes \mathbf{q}^{-1}=\mathbf{q}^{-1} \otimes \mathbf{q}=\mathbf{q}_{1}$, $\mathbf{q}^{-1}=\mathbf{q}^{*} /\|\mathbf{q}\|^{2}$ 



###### 1.2.4 单位四元数

当$\Vert \mathbf{q} \Vert=1$的时候，满足: $\mathbf{q}^{-1}=\mathbf{q}^{*}$，单位四元数总是可以写成：$\mathbf{q}=\left[\begin{array}{c}{\cos \theta} \\ {\mathbf{u} \sin \theta}\end{array}\right]$ 其中$\mathbf{u}=u_{x} i+u_{u} j+u_{z} k$

##### 1.3 补充性质

###### 1.3.1 交换子(commutator)

定义如下：
$$
[\mathbf{p}, \mathbf{q}] \triangleq \mathbf{p} \otimes \mathbf{q}-\mathbf{q} \otimes \mathbf{p}
$$
$$
\mathbf{p} \otimes \mathbf{q}-\mathbf{q} \otimes \mathbf{p}=2 \mathbf{p}{v} \times \mathbf{q}{v}
$$

###### 1.3.2 纯虚四元数的乘积

$$
Q=\mathbf{q}_{v}, \quad \mathbf{q}=\left[0, \mathbf{q}_{v}\right]
$$

$$
\mathbf{p}_{v} \otimes \mathbf{q}_{v}=-\mathbf{p}_{v}^{\top} \mathbf{q}_{v}+\mathbf{p}_{v} \times \mathbf{q}_{v}=\left[\begin{array}{c}{-\mathbf{p}_{v}^{\top} \mathbf{q}_{v}} \\ {\mathbf{p}_{v} \times \mathbf{q}_{v}}\end{array}\right]
$$

可以推出:
$$
\mathbf{q}_{v} \otimes \mathbf{q}_{v}=-\mathbf{q}_{v}^{\top} \mathbf{q}_{v}=-\left\|\mathbf{q}_{v}\right\|^{2}
$$
对于：$\mathbf{u} \in \mathbb{H}_{p},\|\mathbf{u}\|=1$，有:$\mathbf{u} \otimes \mathbf{u}=-1$



###### 1.3.3 纯虚四元数的指数映射$\Rightarrow$单位四元数

利用:
$$
\mathbf{v}^{2}=-\theta^{2} \quad, \quad \mathbf{v}^{3}=-\mathbf{u} \theta^{3} \quad, \quad \mathbf{v}^{4}=\theta^{4} \quad, \quad \mathbf{v}^{5}=\mathbf{u} \theta^{5} \quad, \quad \mathbf{v}^{6}=-\theta^{6} \quad, \quad \cdots
$$
与:
$$
e^{\mathbf{q}} \triangleq \sum_{k=0}^{\infty} \frac{1}{k !} \mathbf{q}^{k} \in \mathbb{H}
$$


于是可以得到纯虚四元数的指数映射为：
$$
e^{\mathbf{u} \theta}=\left(1-\frac{\theta^{2}}{2 !}+\frac{\theta^{4}}{4 !}+\cdots\right)+\left(\mathbf{u} \theta-\frac{\mathbf{u} \theta^{3}}{3 !}+\frac{\mathbf{u} \theta^{5}}{5 !}+\cdots\right)
$$
即：
$$
e^{\mathrm{v}}=e^{\mathbf{u} \theta}=\cos \theta+\mathbf{u} \sin \theta=\left[\begin{array}{c}{\cos \theta} \\ {\mathbf{u} \sin \theta}\end{array}\right]
$$
形式与欧拉公式$e^{i\theta}=\cos\theta+i\sin\theta$类似.

> 特殊性质：$e^{-\mathrm{v}}=\left(e^{\mathrm{v}}\right)^{*}$

`小角度近似`：为了避免除$0$，当$\mathrm{v}$较小时，有如下近似
$$
e^{\mathbf{v}} \approx\left[\begin{array}{c}{1-\theta^{2} / 2} \\ {\mathbf{v}\left(1-\theta^{2} / 6\right)}\end{array}\right] \approx\left[\begin{array}{l}{1} \\ {\mathbf{v}}\end{array}\right] \underset{\theta \rightarrow 0}{\longrightarrow}\left[\begin{array}{l}{1} \\ {0}\end{array}\right]
$$

###### 1.3.4 普通四元数的指数映射

$$
e^{\mathbf{q}}=e^{q_{w}}\left[\begin{array}{c}{\cos \left\|\mathbf{q}_{v}\right\|} \\ {\frac{\mathbf{q}_{v}}{\left\|\mathbf{q}_{v}\right\|} \sin \left\|\mathbf{q}_{v}\right\|}\end{array}\right]
$$

###### 1.3.5 单位四元数的对数映射$\Rightarrow$纯虚四元数
当满足$\|\mathbf{q}\|=1$时，有：
$$
\log \mathbf{q}=\log (\cos \theta+\mathbf{u} \sin \theta)=\log \left(e^{\mathbf{u} \theta}\right)=\mathbf{u} \theta=\left[\begin{array}{c}{0} \\ {\mathbf{u} \theta}\end{array}\right]
$$
$$
\begin{aligned} \mathbf{u} &=\mathbf{q}_{v} /\left\|\mathbf{q}_{v}\right\| \\ \theta &=\arctan \left(\left\|\mathbf{q}_{v}\right\|, q_{w}\right) \end{aligned}
$$

`小角度近似`:
$$
\log (\mathbf{q})=\mathbf{u} \theta=\mathbf{q}_{v} \frac{\arctan \left(\left\|\mathbf{q}_{v}\right\|, q_{w}\right)}{\left\|\mathbf{q}_{v}\right\|} \approx \frac{\mathbf{q}_{v}}{q_{w}}\left(1-\frac{\left\|\mathbf{q}_{v}\right\|^{2}}{3 q_{w}^{2}}\right) \approx \mathbf{q}_{v} \underset{\theta \rightarrow 0}{\longrightarrow} \mathbf{0}
$$

###### 1.3.6　$\mathbf{q}^{t}$的指数形式

$$
\mathbf{q}^{t}=\exp \left(\log \left(\mathbf{q}^{t}\right)\right)=\exp (t \log (\mathbf{q}))
$$

如果$\|\mathbf{q}\|=1$,可以写作：$\mathbf{q}=[\cos \theta, \mathbf{u} \sin \theta]$,$\log (\mathbf{q})=\mathbf{u} \theta$ 于是可以得到：
$$
\mathbf{q}^{t}=\exp (t \mathbf{u} \theta)=\left[\begin{array}{c}{\cos t \theta} \\ {\mathbf{u} \sin t \theta}\end{array}\right]
$$
由于$t$与$\theta$结合在了一起，可以利用这个性质完成四元数的线性插值．