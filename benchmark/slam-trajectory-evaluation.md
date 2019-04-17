[TOC]

## SLAM 轨迹评估指标以及具体求解方法

### 常用的指标

#### ATE(Absolute Trajectory Error) 绝对轨迹误差

> 比较适合评估SLAM算法的性能

$$
ATE_{all} = \sqrt{\frac{1}{N}\sum_{i=1}^N\Vert \log(T_{gt,i}^{-1}T_{esti,i})^{\vee} \Vert^2_2 } \\
$$

上式其实际上是每个Pose的李代数的均方根误差(Root-Mean-Squared-Error,RMSE)。可以用来表示两条轨迹的旋转和平移误差。有些文献只考虑平移误差，即可得到下列式子：
$$
ATE_{trans} = \sqrt{\frac{1}{N}\sum_{i=1}^{N}\Vert trans(T_{gt,i}^{-1}T_{esti,i}) \Vert^2_{2} }
$$
其中$trans()$表示取括号内变量的平移部分，从整条轨迹来看，旋转出现误差后，随后的轨迹在平移上也会出现误差，所以两种指标在实际中都适用。

#### RPE(Relative Pose Error) 相对位姿误差

> 比较适合评估VO算法，例如Drift每秒


$$
RPE_{all} = \sqrt{\frac{1}{N-1}\sum_{i=1}^{N-1}\Vert \log((T_{gt,i}^{-1}T_{gt,i+\Delta t})^{-1}
(T_{esti,i}^{-1}T_{esti,i+\Delta t}))^{\vee} \Vert_2^{2} }
$$
同样，如果只去平移部分可以写成如下形式：
$$
RPE_{all} = \sqrt{\frac{1}{N-1}\sum_{i=1}^{N-1}\Vert trans((T_{gt,i}^{-1}T_{gt,i+\Delta t})^{-1}
(T_{esti,i}^{-1}T_{esti,i+\Delta t})) \Vert_2^{2} }
$$



### 具体操作流程

实际操作时候需要进行一系列的预处理工作，例如：轨迹的时间对齐，外参预估等。