### 贝叶斯滤波与粒子滤波

#### Structure of Robotics Problems

+ Time: $t$
+ Robot State: $x_t$
+ Control Input: $u_t$
+ Observation: $z_t$
+ Environment State: $m_t$

控制输入$u_{0:t}$和观测值$z_{0:t}$假设已知，机器人状态$x_{0:t}$和环境状态$m_{0:t}$假定未知．

`Markov Assumptions` :

+ 状态$x_{t+1}$只和前一个状态$x_t$以及控制输入$u_t$有关
+ 观测$z_t$只依赖机器人当前状态$x_t$以及$m_t$

`运动模型`
$$
x_{t+1}=f\left(x_{t}, u_{t}, w_{t}\right) \sim p_{f}\left(\cdot | x_{t}, u_{t}\right) 
$$
$ w_{t}$为运动噪声．

`观测模型`
$$
z_{t}=h\left(x_{t}, m_{t}, v_{t}\right) \sim p_{h}\left(\cdot | x_{t}, m_{t}\right)
$$
$v_t$为观测噪声.

#### 贝叶斯滤波

基于Markov假设和贝叶斯法则，把控制输入和观测结果结合起来，估计机器人的状态．

+ 全概率公式：$p(x)=\int p(x, y) d y$
+ 条件概率：$p(x,y)=p(y|x)p(x)$
+ 贝叶斯公式：$p(x|y,z)=p(x | y, z)=\frac{p(y | x, z) p(x | z)}{\int p(y, s | z) d s}=\frac{p(y | x, z) p(z | x) p(x)}{p(y | z) p(z)}$

贝叶斯滤波的特殊情况：

+ $Kalman$滤波
+ 粒子滤波
+ Forward Algorithm for Hidden Markov Models(HMMs)



