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

