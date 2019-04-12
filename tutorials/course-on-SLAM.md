[TOC]
# Course on SLAM 
> Joan Sola

## Robot Motion
### 刚体坐标变换
#### 坐标系相关符号规定
`坐标系B中的点与向量`:$\mathbf{p}_{B}, \mathbf{v}_{B}, \mathbf{p}^{B}, \mathbf{v}^{B}$
`全局坐标系中的点与向量`：$\mathbf{p}, \mathbf{v}$
`坐标系B相对于F的位姿`：$B_{F}=\left(\mathbf{t}_{F B}, \mathbf{\Phi}_{F B}\right)$
`局部坐标系相对于全局坐标系的位姿`：$(\mathbf{t}, \Phi)$

> ps:个人理解B相对于F的位姿态，即对应B系点变换到F系的变换矩阵，即平移对应F系原点到B系原点向量在F系下的坐标，旋转为由F系旋转到B系对应的旋转矩阵。eg:由F经z-y-x顺序旋转到B，对应旋转矩阵为$R_{FB}=R_zR_yR_x$

`2D`：
$$
B=\left[ \begin{array}{l}{\mathbf{t}} \\ {\theta}\end{array}\right] \in \mathbb{R}^{3}
$$
`3D`：
$$
B=\left[ \begin{array}{c}{\mathbf{t}} \\ {\mathbf{q}}\end{array}\right] \in \mathbb{R}^{7}, \quad\|\mathbf{q}\|=1
$$
其中$\mathbf{q}=\left[q_{w}, q_{x}, q_{y}, q_{z}\right]^{\top}$

#### 坐标系变换

不同坐标系之间的点与向量之间的变换，假设载体坐标系为：$B=(\mathbf{t}, \Phi)$

则对应的点坐标变换为：
$$
\begin{aligned} \mathbf{p} &=\mathbf{R}\{\mathbf{\Phi}\} \mathbf{p}_{B}+\mathbf{t} \\ \mathbf{v} &=\mathbf{R}\{\mathbf{\Phi}\} \mathbf{v}_{B} \end{aligned}
$$
对应的向量变换为：
$$
\begin{aligned} \mathbf{p}_{B} &=\mathbf{R}\{\mathbf{\Phi}\}^{\top}(\mathbf{p}-\mathbf{t}) \\ \mathbf{v}_{B} &=\mathbf{R}\{\mathbf{\Phi}\}^{\top} \mathbf{v} \end{aligned}
$$


其中$\mathbf{R}\{\mathbf{\Phi}\}^{\top}$为$\mathbf{\Phi}$对应的旋转矩阵。

`2D`:
$$
\mathbf{R}\{\theta\}=\left[ \begin{array}{cc}{\cos \theta} & {-\sin \theta} \\ {\sin \theta} & {\cos \theta}\end{array}\right]
$$
`3D`:
$$
\mathbf{R}\{\mathbf{q}\}=\left[ \begin{array}{ccc}{q_{w}^{2}+q_{x}^{2}-q_{y}^{2}-q_{z}^{2}} & {2\left(q_{x} q_{y}-q_{w} q_{z}\right)} & {2\left(q_{x} q_{z}+q_{w} q_{y}\right)} \\ {2\left(q_{x} q_{y}+q_{w} q_{z}\right)} & {q_{w}^{2}-q_{x}^{2}+q_{y}^{2}-q_{z}^{2}} & {2\left(q_{y} q_{z}-q_{w} q_{x}\right)} \\ {2\left(q_{x} q_{z}-q_{w} q_{y}\right)} & {2\left(q_{y} q_{z}+q_{w} q_{x}\right)} & {q_{w}^{2}-q_{x}^{2}-q_{y}^{2}+q_{z}^{2}}\end{array}\right]
$$

#### 坐标系之间的结合

给定两个坐标系：R系和S系，则S相对于R的位姿态可以通过如下方式表示：
$$
R=\left[ \begin{array}{c}{\mathbf{t}_{R}} \\ {\mathbf{\Phi}_{R}}\end{array}\right] \quad, \quad S=\left[ \begin{array}{c}{\mathbf{t}_{S}} \\ {\mathbf{\Phi}_{S}}\end{array}\right] \quad, \quad S_{R}=\left[ \begin{array}{c}{\mathbf{t}_{R S}} \\ {\mathbf{\Phi}_{R S}}\end{array}\right]
$$
`2D`:
$$
\begin{aligned} 
S&= \left[ \begin{array}{c}{\mathbf{t}_{S}} \\ 
{\theta_{S}}\end{array}\right] =\left[ \begin{array}{c}{\mathbf{t}_{R}+\mathbf{R}\left\{\theta_{R}\right\} \mathbf{t}_{R S}} \\ {\theta_{R}+\theta_{R S}}\end{array}\right] \\
S_{R}&=\left[ \begin{array}{c}{\mathbf{t}_{R S}}\\
{\theta_{R S}}\end{array}\right]=\left[ \begin{array}{c}{\mathbf{R}\left\{\theta_{R}\right\}^{\top}\left(\mathbf{t}_{S}-\mathbf{t}_{R}\right)} \\
{\theta_{S}-\theta_{R}}\end{array}\right]
\end{aligned}
$$
`3D`:
$$
\begin{aligned} S&= \left[ \begin{array}{c}{\mathbf{t}_{S}} \\ {\mathbf{q}_{S}}\end{array}\right] =\left[ \begin{array}{c}{\mathbf{t}_{R}+\mathbf{R}\left\{\mathbf{q}_{R}\right\} \mathbf{t}_{R S}} \\ {\mathbf{q}_{R} \otimes \mathbf{q}_{R S}}\end{array}\right] \\ S_{R}&=\left[ \begin{array}{c}{\mathbf{t}_{R S}} \\ {\mathbf{q}_{R S}}\end{array}\right]=\left[ \begin{array}{c}{\mathbf{R}\left\{\mathbf{q}_{R}\right\}^{\top}\left(\mathbf{t}_{S}-\mathbf{t}_{R}\right)} \\ {\mathbf{q}_{R}^{*} \otimes \mathbf{q}_{S}}\end{array}\right] \end{aligned}
$$


其中$q^{*}$为单位四元数$q$的共轭。

### 机器人运动建模

机器人的状态为一个关于：上一时刻状态、控制输入、噪声的函数：
$$
\mathbf{x}_{n}=f_{n}\left(\mathbf{x}_{n-1}, \mathbf{u}_{n}, \mathbf{i}\right), \quad \mathbf{i} \sim \mathcal{N}\{0, \mathbf{Q}\}
$$
也可以记做：
$$
\mathbf{x} \leftarrow f(\mathbf{x}, \mathbf{u}, \mathbf{i}), \quad \mathbf{i} \sim \mathcal{N}\{0, \mathbf{Q}\}
$$

#### 不确定性的传播

假定机器人状态分布为高斯分布：$\mathbf{x} \sim \mathcal{N}\{\overline{\mathbf{x}}, \mathbf{P}\}$

则对应的更新方程为：
$$
\begin{aligned} \overline{\mathbf{x}} \leftarrow & f(\overline{\mathbf{x}}, \mathbf{u}, 0) \\ \mathbf{P}  \leftarrow& \mathbf{F}_{x} \mathbf{P} \mathbf{F}_{x}^{\top}+\mathbf{F}_{i} \mathbf{Q} \mathbf{F}_{i}^{\top} \end{aligned}
$$


其中$\overline{\mathbf{x}}$为$\mathbf{x}$的均值，$\mathbf{P}$为其协方差矩阵，$\mathbf{i}$为扰动。
$$
\mathbf{F}_{x}=\left.\frac{\partial f}{\partial \mathbf{x}}\right|_{\overline{\mathbf{x}}, \mathbf{u}, \mathbf{i}=0}, \quad \mathbf{F}_{i}=\left.\frac{\partial f}{\partial \mathbf{i}}\right|_{\overline{\mathbf{x}}, \mathbf{u}, \mathbf{i}=0}
$$


### 常见的运动模型

#### 匀速运动模型

> 适用于控制输入无法获得的时候，例如手持相机运动

外部的各种因素最终造成的扰动会直接表现在速度上$\mathbf{v}_{i} ,\boldsymbol{\omega}_{i}$
$$
\mathbf{x}=\left[ \begin{array}{c}{\mathbf{p}} \\ {\mathbf{v}} \\ {\mathbf{q}} \\ {\boldsymbol{\omega}}\end{array}\right], \quad \mathbf{i}=\left[ \begin{array}{c}{\mathbf{v}_{i}} \\ {\boldsymbol{\omega}_{i}}\end{array}\right]
$$

$$
\begin{array}{l}{\mathbf{p} \leftarrow \mathbf{p}+\mathbf{v} \delta t} \\ {\mathbf{v} \leftarrow \mathbf{v}+\mathbf{v}_{i}} \\ {\mathbf{q} \leftarrow \mathbf{q} \otimes \mathbf{q}\{\omega \delta t\}} \\ {\boldsymbol{\omega} \leftarrow \boldsymbol{\omega}+\boldsymbol{\omega}_{i}}\end{array}
$$

#### 匀加速度模型

> 适用于运动比较平滑的时候，因为加速度无法突变

$$
\mathbf{x}=\left[ \begin{array}{c}{\mathbf{p}} \\ {\mathbf{v}} \\ {\mathbf{a}} \\ {\mathbf{q}} \\ {\boldsymbol{\omega}} \\ {\boldsymbol{\alpha}}\end{array}\right],\mathbf{i}=\left[ \begin{array}{c}{\mathbf{a}_{i}} \\ {\boldsymbol{\alpha}_{i}}\end{array}\right]
$$



更新方式：
$$
\begin{array}{l}{\mathbf{p} \leftarrow \mathbf{p}+\mathbf{v} \delta t+\frac{1}{2} \mathbf{a} \delta t^{2}} \\ {\mathbf{v} \leftarrow \mathbf{v}+\mathbf{a} \delta t} \\ {\mathbf{a} \leftarrow \mathbf{a}+\mathbf{a}_{i}} \\ {\mathbf{q} \leftarrow \mathbf{q} \otimes \mathbf{q}\left\{\boldsymbol{\omega} \delta t+\frac{1}{2} \boldsymbol{\alpha} \delta t^{2}\right\}} \\ {\boldsymbol{\omega} \leftarrow \boldsymbol{\omega}+\boldsymbol{\alpha} \delta t} \\ {\boldsymbol{\alpha} \leftarrow \boldsymbol{\alpha}+\boldsymbol{\alpha}_{i}}\end{array}
$$


#### 里程计模型

> 适用于轮式和腿型机器人，控制信号$\mathbf{u}$直接作用于机器人当前状态，带来位姿的局部增量，通过对微小增量的积分可以得到完整的轨迹。

`2D`

控制信号为：$\mathbf{u}=[\delta \mathbf{p}, \delta \theta] \in \mathbb{R}^{3}$
$$
\mathbf{x}=\left[ \begin{array}{c}{\mathbf{p}} \\ {\theta}\end{array}\right], \quad \mathbf{u}=\left[ \begin{array}{c}{\delta \mathbf{p}} \\ {\delta \theta}\end{array}\right], \quad \mathbf{i}=\left[ \begin{array}{c}{\delta \mathbf{p}_{i}} \\ {\delta \theta_{i}}\end{array}\right]
$$


模型更新为：$\mathbf{x} \leftarrow f(\mathbf{x}, \mathbf{u}, \mathbf{i})$
$$
\begin{array}{l}{\mathbf{p} \leftarrow \mathbf{p}+\mathbf{R}\{\theta\}\left(\delta \mathbf{p}+\delta \mathbf{p}_{i}\right)} \\ {\theta \leftarrow \theta+\delta \theta+\delta \theta_{i}}\end{array}
$$
对应的$Jacobian$矩阵为：
$$
\mathbf{F}_{x}=\left[ \begin{array}{ccc}{1} & {0} & {(-d x \sin \theta-d y \cos \theta)} \\ {0} & {1} & {(d x \cos \theta-d y \sin \theta)} \\ {0} & {0} & {1}\end{array}\right], \quad \mathbf{F}_{i}=\left[ \begin{array}{ccc}{\cos \theta} & {-\sin \theta} & {0} \\ {\sin \theta} & {\cos \theta} & {0} \\ {0} & {0} & {1}\end{array}\right]
$$

> 推导过程，展开即可:$\delta \mathbf{p} =(\delta x,\delta y)^{\top}$

`3D`

控制信号为：$\mathbf{u}=[\delta \mathbf{p}, \delta \boldsymbol{\theta}] \in \mathbb{R}^{6}$
$$
\mathbf{x}=\left[ \begin{array}{c}{\mathbf{p}} \\ {\mathbf{q}}\end{array}\right], \quad \mathbf{u}=\left[ \begin{array}{c}{\delta \mathbf{p}} \\ {\delta \boldsymbol{\theta}}\end{array}\right], \quad \mathbf{i}=\left[ \begin{array}{c}{\delta \mathbf{p}_{i}} \\ {\delta \boldsymbol{\theta}_{i}}\end{array}\right]
$$

$$
\begin{array}{l}{\mathbf{p} \leftarrow \mathbf{p}+\mathbf{R}\{\mathbf{q}\}\left(\delta \mathbf{p}+\delta \mathbf{p}_{i}\right)} \\ {\mathbf{q} \leftarrow \mathbf{q} \otimes \mathbf{q}\left\{\delta \boldsymbol{\theta}+\delta \boldsymbol{\theta}_{i}\right\}}\end{array}
$$

对应的坐标系更新：
$$
\mathbf{x} \leftarrow \mathbf{x} \oplus\left[\delta \mathbf{p}+\delta \mathbf{p}_{i}, \mathbf{q}\left\{\delta \boldsymbol{\theta}+\delta \boldsymbol{\theta}_{i}\right\}\right]
$$


#### 差速轮模型(2D)

> 两个轮子的移动机器人，且机器人中心为两个轮子轴中心

模型的主要参数有两个：

+ 轮子之间的距离$d$
+ 轮子半径$r$

测量值主要通过两个轮子上的编码器获得的角度增量来计算。在时间间隔$  \delta t$内，左右轮角度增量为：$\delta \psi_{L}, \delta \psi_{R}$。当时间间隔$\delta t$较小的时候，对应的角度增量$\delta \theta$也为一个小量，左右轮共同作用：

+ 共模分量：导致$x$方向上的平移
+ 差模分量：导致$z$轴方向的旋转

`数学模型`如下：
$$
\begin{aligned} \delta x &=\frac{r\left(\delta \psi_{R}+\delta \psi_{L}\right)}{2} \\ \delta y &=0 \\ \delta \theta &=\frac{r\left(\delta \psi_{R}-\delta \psi_{L}\right)}{d} \end{aligned}
$$
对应的2D控制步长为：
$$
\mathbf{u}=\left[ \begin{array}{c}{\delta \mathbf{p}} \\ {\delta \theta}\end{array}\right]=\left[ \begin{array}{l}{\delta x} \\ {\delta y} \\ {\delta \theta}\end{array}\right]
$$
对其进行积分即可获取其轨迹，协方差$\mathbf{Q}$ 可以通过测量的角度获得。
$$
\mathbf{Q}=\mathbf{J} \mathbf{Q}_{\psi} \mathbf{J}^{\top}
$$
其中$\mathbf{J}$为上述数学模型的$Jacobian$矩阵，$\mathbf{Q_{\psi}}$为轮子编码器测量角度的协方差矩阵。
$$
\mathbf{J}=\left[ \begin{array}{cc}{r / 2} & {r / 2} \\ {0} & {0} \\ {r / d} & {-r / d}\end{array}\right]
,\mathbf{Q}_{\psi}=\left[ \begin{array}{cc}{\sigma_{\psi}^{2}} & {0} \\ {0} & {\sigma_{\psi}^{2}}\end{array}\right]
$$
