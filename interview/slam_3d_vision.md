知乎: [wlwsjl](https://www.zhihu.com/people/wlwsjl)

接着上次关于[求职经历](https://zhuanlan.zhihu.com/p/56617825)的介绍，下面记录下之前笔试面试碰到的一些问题，有一些纯粹是瞎聊（这个有可能扛不住=_=）。由于时间有点久远，好些已经记不得了，再不记就要忘光了，往后憋毕设估计也没有心思整理了。只记问题~大部分网上都有答案，也可以参看第六部分。另外，因为我有些项目涉及机器学习、深度学习部分所以面试也有所涉及，这部分对于SLAM求职者来说没必要花费太多精力。

> **一：程序基础**
> **二：数学基础**
> **三：SLAM**
> **四：传统图像处理**
> **五：机器学习以及深度学习**
> **六：参考资料**

## 一：程序基础

**考察C++、数据结构**

1. 多线程的了解
2. stl有什么？
3. vector扩充方式，size与capacity区别
4. 顺序存储结构有哪些？
5. 左值引用与右值引用
6. map与unordered map区别
7. const与static、const在函数前与函数后区别
8. 虚函数与纯虚函数区别，虚函数关键字
9. 函数memcpy 、memset的实现，手撕代码
10. 一行代码求平方根
11. 各种排序时间空间复杂度（快排，归并，桶排，堆排），手撕代码
12. 二叉树排序、堆排序、希尔排序、桶排序时间复杂度（重要！因此重复）
13. 最长公共子串、最长公共子序列，手撕代码
14. 树的DFS与BFS、树的遍历，手撕代码
15. 对于n个实例的k维数据，建立kd tree的时间复杂度
16. 哈夫曼树带权路径长度、哈夫曼编码
17. 长度为n的list，删除、插入与随机访问的计算复杂度
18. 在有序表中二分查找关键字所需进行的关键字比较次数是多少？
19. 字符串子串数目
20. 三维空间最近邻搜索的常用数据结构（八叉树、kd tree）
21. HashMap和Hashtable的比较
22. 在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数

> 10的答案（张同学的同学很给力）。放到10下面11就变成1了=_=，因此放到最后。
> import math
> print([n for n in range(2,X) if 0 not in [n%d for d in range(2,int(math.sqrt(n))+1)]])
> 如果import不算的话…

## 二：数学基础

**考察概率论、线性代数、矩阵分析、数值优化**

1. 一层楼共有n级台阶，一次可以上至少一级但不超过m级台阶，求有多少种不同的上楼方案数。由于结果可能很大，你只需要输出结果对10007取模的值即可
2. 拟合二维平面中的带噪声直线，其中有不超过20%的样本点远离了直线，另外80%的样本点可能有高斯噪声的偏移，要求输出为ax+by+c=0的形式，其中a>0且a^2+b^2=1
3. 切比雪夫不等式、协方差与相关系数、各种分布、多元高斯分布
4. 线性回归推导回归系数（y=kx，y=kx+b）
5. 甲乙两人约好在某地碰面，时间段为10点到11点。若甲先到，最多会等待10分钟，10分钟内乙未出现则离开；若乙先到，最多会等待15分钟，15分钟内甲未出现则离开，请问两人见面的概率是多少？
6. ABCDE5个人互相传球，由A开始第一次传球，经5次传球最后回到A的手上，其中A与B不会互相传球，C只会传给D，E不会传给C，共有多少种传球方法？
7. 平均要取多少个(0,1)中的随机数才能让和超过1
8. MLE、MAP和贝叶斯估计
9. MLE，MAP，EM 和 point estimation 之间的关系是怎样的？
10. 如何求解Ax=b (非迭代Cholesky分解、QR分解，迭代)
11. 最小二乘封闭解与迭代解的取舍
12. 梯度下降法、牛顿法、GN、LM，推导、优缺点
13. 如何判断点在多边形内
14. 一阶、二阶优化，Jacobian、hessian矩阵
15. 1000个数的阶乘，求有多少个0
16. 递推法求数学期望，反证法，数学归纳法等

## 三：SLAM

**这部分除了3D视觉基础会结合个人研究方向问，对于泛泛的问题尽量发散**

1. landmark参数化方式、对比，逆深度参数化；点线面因子图优化
2. 滤波+回环（Trifo-VIO）
3. outlier+鲁棒核、RANSAC
4. EKF更新方程
5. AR系统如何实现
6. 介绍下VO
7. Gridmap（网格标0、1）给定起点和终点，求最优路径（A*或其他路径规划算法）
8. 相似变换、仿射变换、射影变换的区别
9. E和F的区别，自由度计算
10. 单应矩阵H的求取
11. PNP算法、ICP算法（二维码、手眼标定）
12. 闭环检测常用方法（orb、lsd、深度学习）
13. 单目的初始化（拓展：双目，RGBD，VIO的初始化及传感器标定），其他：[https://github.com/frobelbest/GSLAM](https://link.zhihu.com/?target=https%3A//github.com/frobelbest/GSLAM)
14. 简述一下Bundle Adjustment的过程
15. SVO、LSD中深度滤波器原理
16. 说一说某个SLAM框架的工作原理（svo、orb、lsd）及其优缺点，如何改进？
17. RANSAC的框架
18. 位姿不同表示间的相互转化、旋转矩阵特征值和特征向量物理意义
19. 真实世界到相机照片的变换可看成射影变换
20. 直接法与特征点法的优缺点对比
21. 常见滤波方法的对比（KF、EKF、IEKF、UKF、PF）
22. 双目测距范围Z=fb/d。问题： 640*480，fov＝90°，zmax＝10m，最小视差为2，求使zmax稳定的最小基线长度（6.25cm）
23. 特征点法与直接法误差模型、Jacobian推导
24. 光流的假设、仿射变换、4种方法，svo采取的方法，优势何在
25. MSCKF与ROVIO、MSCKF与预积分（structureless factor）
26. 边缘化方式原理
27. grid_map [https://github.com/ANYbotics/grid_map](https://link.zhihu.com/?target=https%3A//github.com/ANYbotics/grid_map)

## 四：传统图像处理

1. 图像平滑算子、边缘检测算子
2. 图像去噪滤波算法（高斯、均值、双边、Guide filter）
3. 三个度量patch相似度的方法（SSD、SAD、NCC）
4. 二进制描述子
5. 计算描述子距离函数
6. 描述一下SIFT或者SURF特征检测、匹配
7. SIFT的4个不变性
8. 特征点、描述子ORB、SIFT、SURF、BRIEF等等 。geometric invariance：平移，旋转，尺度……; photometric invariance：亮度，曝光……
9. Mat实现、Mat类指针引用复制函数
10. 颜色直方图统计，手撕代码
11. 形态学操作，手撕代码
12. 积分图，手撕代码
13. 连通区域算法，给二值图，求出最大联通区域（用深度优先和广度优先算法，手撕代码）
14. Mser、Swt检测
15. 图像分割（Grabcut）
16. 目标跟踪（相关滤波KCF）

## 五：机器学习以及深度学习

**这部分会很随意，根据项目**

1. IOU、NMS，手撕代码
2. Kmeans伪代码
3. SVM的优缺点
4. 随机森林的训练过程
5. 优化方法SGD、Batch GD、Adadelta、Momentum对超参数的敏感程度
6. CNN中feature map维度计算、图中每一个特征点在原图的感受野大小
7. Segmatch
8. 目标分割、目标检测（one stage、two stage），YOLO三代的发展，小目标检测
9. 模型压缩与加速 mobilenet v1、mobilenet v2、shufflenet

[https://github.com/memoiry/Awesome-model-compression-and-acceleration](https://link.zhihu.com/?target=https%3A//github.com/memoiry/Awesome-model-compression-and-acceleration)

## 六：参考资料

大疆算法工程师笔试（计算机视觉部分）

[https://wenku.baidu.com/view/9aacf48ea21614791611282a.html](https://link.zhihu.com/?target=https%3A//wenku.baidu.com/view/9aacf48ea21614791611282a.html)

网易3D视觉方向

[https://www.nowcoder.com/test/10780247/summary](https://link.zhihu.com/?target=https%3A//www.nowcoder.com/test/10780247/summary)

谢晓佳学长“SLAM求职经验帖”

[http://paopaorobot.org/bbs/read.php?tid=87&fid=7](https://link.zhihu.com/?target=http%3A//paopaorobot.org/bbs/read.php%3Ftid%3D87%26fid%3D7)

[http://paopaorobot.org/bbs/read.php?tid=88&fid=7](https://link.zhihu.com/?target=http%3A//paopaorobot.org/bbs/read.php%3Ftid%3D88%26fid%3D7)

**VO、SLAM、VIO基础论文**

> VO

[http://rpg.ifi.uzh.ch/visual_odometry_tutorial.html](https://link.zhihu.com/?target=http%3A//rpg.ifi.uzh.ch/visual_odometry_tutorial.html)

《VO_Part_I_Scaramuzza》、《VO_Part_II_Scaramuzza》

> 光流

《Lucas-Kanade 20 Years On: A Unifying Framework》

> 图优化SLAM

《A Tutorial on Graph-Based SLAM》

《g2o: A General Framework for Graph Optimization》

> 参数化

《Impact of Landmark Parametrization on Monocular EKF-SLAM with Points and Lines》

> 边缘化

《Null-Space-based Marginalization》

> VIO

《Quaternion kinematics for the error-state Kalman filter》

《Information Sparsification in Visual-Inertial Odometry》

**其他常见SLAM、VIO系统相关论文（结合自己研究方向）**

**综述性质论文**

下面第一篇论文总结了如下几点，感觉总结的很好

- 数据关联
- 初始化
- 位姿估计
- 地图生成
- 地图维护
- 失效恢复
- 回环检测

《Keyframe-based monocular SLAM: design, survey, and future directions》

《Past, Present, and Future of Simultaneous Localization And Mapping: Towards the Robust-Perception Age》

《Visual Place Recognition: A Survey》

《Simultaneous Localization and Mapping: A Survey of Current Trends in Autonomous Driving》

《Visual SLAM and Structure from Motion in Dynamic Environments: A Survey》

《A Benchmark Comparison of Monocular Visual-Inertial Odometry Algorithms for Flying Robots》

《GSLAM: A General SLAM Framework and Benchmark》

[https://github.com/zdzhaoyong/GSLAM](https://link.zhihu.com/?target=https%3A//github.com/zdzhaoyong/GSLAM)

《Survey on Computer Vision for UAVs: Current Developments and Trends》

《Semantic mapping for mobile robotics tasks: A survey》

《A Review on Deep Learning Techniques Applied to Semantic Segmentation》

《Deep Learning for Generic Object Detection: A Survey》

**轨迹评估算法**

[https://github.com/MichaelGrupp/evo](https://link.zhihu.com/?target=https%3A//github.com/MichaelGrupp/evo)

[https://github.com/raulmur/evaluate_ate_scale](https://link.zhihu.com/?target=https%3A//github.com/raulmur/evaluate_ate_scale)

ROVIO：[https://github.com/ethz-asl/trajectory_toolkit](https://link.zhihu.com/?target=https%3A//github.com/ethz-asl/trajectory_toolkit)

SVO：[https://github.com/uzh-rpg/rpg_svo/tree/master/svo_analysis](https://link.zhihu.com/?target=https%3A//github.com/uzh-rpg/rpg_svo/tree/master/svo_analysis)

《A Tutorial on Quantitative Trajectory Evaluation for Visual(-Inertial) Odometry》

**VO、SLAM、VIO参考书籍**

《SLAM十四讲》、《因子图在SLAM中的应用》

《state estimation for robotics》

《An Invitation to 3D Vision》、《Multi View Geometry》

**深度学习参考**

《解析卷积神经网络——深度学习实践手册》 [http://lamda.nju.edu.cn/weixs/](https://link.zhihu.com/?target=http%3A//lamda.nju.edu.cn/weixs/)

《神经网络与深度学习》 [https://nndl.github.io/](https://link.zhihu.com/?target=https%3A//nndl.github.io/)

Shirley Snow 刘同学总结 <https://zhuanlan.zhihu.com/p/42807023>

《AI算法工程师手册》 <https://zhuanlan.zhihu.com/p/63638229>



公司：

实验室对我的改变挺大的，在进实验室之前以及大四刚进实验室那一年，我一直想做控制，我本科毕设也是做的机械臂控制。在慢慢和师兄们接触后开始转方向了，研一开始转向SLAM。

我是2018年6月底开始断断续续准备找工作的，之前纠结过一段时间要不要准备申请读博，并没有很强的就业意识。之所以是在6月底是因为阿里菜鸟提前批来我们学校招人，当时抱着试一试的心态参加了面试。之后重新规划了一下，下定了找工作的决心。这其实有点仓促，刷题刷的少，准备的并不充分，在之后笔试面试的时候吃了挺大亏。

关于编程基础、数据结构与算法、SLAM相关算法的准备等等，这些就不细讲了，感谢网上的一些大神。下面两个链接我在找工作的时候经常看。真的是很有价值，其中常见问题应该认真准备，比如“连通区域算法”，我就被问过，当时没有认真看就没有回答上要点。

求职：<https://zhuanlan.zhihu.com/p/28565563>

Visual SLAM算法笔记：[https://blog.csdn.net/mulinb/article/details/53421864](https://link.zhihu.com/?target=https%3A//blog.csdn.net/mulinb/article/details/53421864)

下面分不同公司来介绍吧。

我这个领域的基本上投了个遍。加粗的是参加了面试的公司。

百度：找了一个认识的学长内推，当时岗位相关的有自动驾驶和AR，师兄建议我投自动驾驶，投完一直没有动静，应该是简历没过或者实习转正的很多导致没有hc了。

**阿里**：这个我投的是菜鸟的无人驾驶，当时6月份来的挺早的，我过了设在学校的面试，拿到了实习offer，打电话通知我的时候有个小插曲，当时我在参加河北的一个无人机比赛基本上不看手机，加上我有静音的习惯，因此电话一直没接到，第二天忐忑了一整天，还好后来又call回来，这点感谢阿里工作人员的耐心。但是实验室不允许实习，因此这个机会也错失了。

腾讯：找了在腾讯上班的同学内推，但是我估计没找对岗位，笔试题是数据科学类的，很迷，后来就不了了之了。

**网易**：投的是3D 视觉算法工程师，这个做了挺多笔记。笔试题可以参看牛客网上的，考的很全面，专业方面偏向于VIO这块，SVO、因子图优化、预积分、MSCKF考察的比较多。面试需要去杭州现场面，而且只报销600，这个差评。虽然花了不少钱，但是这一趟杭州之行还是受益匪浅。

**大疆**：投的是感知算法工程师，侧重VIO，笔试考的很全，考察基础的图像处理算法以及SLAM14讲里面的经典算法，二面问项目难点以及解决手段。大疆的流程超长，6月份开始投，7月初笔试，7月中下旬一面，8月初二面，8月中三面，9月初才有最终结果。每次等待都很煎熬。

**美团**：无人驾驶算法工程师，主要业务是无人配送。二面同样是现场面试，我不想花钱了就没去。

**360**：人工智能院的SLAM算法工程师，这个面试问了比较多C++的知识，还有就是光流，SVO，滤波。据了解主要是做扫地机器人所以侧重多传感器融合。对了，面试官喜欢出剑指offer上的题目，血亏，没有刷过题！

**旷视**：一面问的是项目，侧重基础理论推导，SVO、LSD等，真的就是边看论文边问问题。二面问项目，侧重深度学习，狂问yolo。面试官直接在电话上问算法题。

**Aibee**：投的是计算机视觉算法工程师。这是一家创业公司，做线下零售，薪资挺诱人的，可能是想做机器人这块所以也招搞SLAM的。听一听宣讲对我们了解公司的发展以及需求还是挺有价值的。面试考察图像处理、3D视觉的基础也问了深度学习的基础。

还有其他相关的公司也可以投，其中一些我也投了但是我签完后就没有参加面试，主要是觉得一轮一轮的面试太累了，而且有的公司应该也不好进，关键是菜，哈哈。

下面列举一下：

华为、OPPO、顺丰、商汤、Momenta、地平线、贝壳、海康威视、景驰科技、图森、pony AI等。

 