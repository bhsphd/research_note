### SLAM算法评估中的轨迹拟合和外参求解

#### 问题描述

两个不同设备采集的轨迹需要进行对齐之后才可以进行算法精度的评估，例如：Vicon采集了一组轨迹，手机运行SLAM算法也获得了一组轨迹，在计算各种误差参数之前必须对轨迹进行对齐。
#### 数学描述

坐标系与符号定义:

+ $w_1$: 手机对应的世界坐标系
+ $w_2$: Vicon 对应的世界坐标系
+ $p$: 手机坐标系(即实时slam输出轨迹)
+ $v$: 固连于手机的Vicon设备对应的局部坐标系
+ $T_{w_1p}$ : 手机获得的轨迹(即变换)
+ $T_{w_2v}$：Vicon设备获得的轨迹
+ $T_{w_2w_1}$ ： 两组轨迹对应的世界坐标系之间的变换
+ $T_{vp}$：手机坐标系到Vicon设备所在局部坐标系的变换

这几个变换之间满足的关系如下：
$$
T_{w_2w_1}T_{w_1p} = T_{w_2v}T_{vp}
$$
其中：$T_{w_2w_1}$和$T_{pv}$为待求解的值，在刚性连接的假设下整个运动过程中为固定值。$T_{w_1p}$和$T_{w_2v}$为已知量，构成两条完整的轨迹，其中的每一帧记做：$T_{w_1p}^{i}$ 和$T_{w_2v}^{j(i)}$ 。

于是我们需要优化的目标函数为：
$$
\arg \min_{T_{w_2w_1},T_{pv}} \sum _i\Vert T_{w_2w_1}T_{w_1p}^{i} - T_{w_2v}^{j(i)}T_{vp} \Vert^2_F
$$
其中$j(i)$为与$i$对应的一帧对应的索引。

上式不好直接优化：

+ 自由度过大
+ 约束不足，$T_{w_2w_1}$和$T_{pv}$都为0显然是一个极小解

#### 求解

##### $T_{w_2w_1}$的求解

假设$T_{vp}$已知，即求解刚体变换将两条轨迹进行对齐(如果是单目轨迹则需要求解一个Sim3变换)。此问题可以通过SVD分解完成高效求解[SVD-Solve-Transform](../math/svd-to-solve-rigid-transform.md)

##### $T_{vp}$的求解

`Way1`使用数值优化算法:
$$
\arg \min _{T_{vp}} \sum_i \Vert  T_{w_2w_1}T_{w_1p}^{i} - T_{w_2v}^{j(i)}T_{vp}  \Vert^2_F
$$
通过把约束项作为惩罚项合并到目标函数中，进行无约束非线性优化.(Why LieGroup?)

`Way2`使用线性解法

`Way3`强行转化成刚体变换问题(求逆即可)

##### 交替优化

求解出$T_{vp}$之后将其带回原式，作为已知量求解$T_{w_2w_1}$，不断进行交替优化。

#### 时间戳对齐

两条轨迹之间时间戳对应关系的求解：

+ way1: 轨迹插值成连续曲线，使用非线性优化求解.[Unified Temporal and Spatial Calibration for Multi-Sensor Systems](https://furgalep.github.io/bib/furgale_iros13.pdf)
+ way2: 搜索算法

`Way2`

假设时间戳误差在$\pm 10s$内以1s为单位，遍历所有可能，把轨迹进行对齐，计算出位置误差(排除姿态误差)最小的那一个。例如:5s处，再在$5s\pm 0.1s$内继续搜索，直到$0.001$为止。在最小分辨率处再使用二次曲线进行拟合，因变量为绝对误差，自变量为时间差。将最小处的值作为最终结果。

算法流程如下：

+ 初始化$T_{vp}$为单位变换
+ 使用$T_{vp}$变换Vicon轨迹后，搜索时间差对齐时间戳
+ 求解$T_{w_2w_1}$ 和$T_{vp}$，少量迭代即可
+ 多次重复步骤2和3
+ 求解轨迹之间的$T_{w_2w_1}$和$T_{vp}$直到误差收敛

#### 代码测试

通过对原始轨迹数据添加两个变换，获得一条新的轨迹，加入噪声(角度和位置)，加入时间差，并对轨迹进行降采样(两个设备频率不同)。

#### 参考

+ [原博客](https://blog.csdn.net/AIchipmunk/article/details/86033405)
+ [SVD最小二乘求解刚体变换](https://igl.ethz.ch/projects/ARAP/svd_rot.pdf)
+ [Quaternion Averaging](https://ntrs.nasa.gov/archive/nasa/casi.ntrs.nasa.gov/20070017872.pdf)
+ [Unified Temporal and Spatial Calibration for Multi-Sensor Systems](https://furgalep.github.io/bib/furgale_iros13.pdf)