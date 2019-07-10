[TOC]

### UZH-FPV Drone Racing Dataset

> SVO那个组整的一个穿越机数据集

#### 1. 介绍

`场景`:

+ 室内
+ 室外
+ 高速飞行

`平台`:

+ miniDAVIS346: Event camera
+ Qualcomm Flight board 
+ Sunny MD-102A VGA Camera  OV7251全局快门传感器
+ IMU与相机硬件同步

`现有数据集`:

+ EuRoc 
+ MVSEC
+ Zurich Urban MAV Dataset



#### 2. 测试结果

室内场景范围:$30m \times 15 m \times 6m$ 

室外场景范围:$80m\times 50m$

现有算法问题:

+ drift
+ scale error

