[TOC]

### 一、基本教程

#### 1. 基本概念

`ceres`可以用于求解如下形式的数学问题：
$$
\begin{split}\min_{\mathbf{x}} &\quad \frac{1}{2}\sum_{i} \rho_i\left(\left\|f_i\left(x_{i_1}, ... ,x_{i_k}\right)\right\|^2\right) \\
\text{s.t.} &\quad l_j \le x_j \le u_j\end{split}
$$
表达式中，$\rho_i\left(\left\|f_i\left(x_{i_1},...,x_{i_k}\right)\right\|^2\right)$被称为残差块`ResidualBlock`，$f_i(\cdot)$被称作`CostFunction`，依赖于参数块`parameter blocks` $[x_{i1},...x_{ik}]$。在大多数优化问题中，有一些标量参数会常常一起出现。因此可以把这几个标量当做一个`ParameterBlock `来处理，例如`slam`中相机的平移(三维向量)和旋转(四元数)都可以当做一个`ParameterBlock `来处理，一个标量显然也是可以看做一个`ParameterBlock `。上式中的$l_j,u_j$为$x_j$取值的范围，公式中的$\rho_i$被称为`LossFunction`，用来减小`outliers`导致的误差过大的问题。

当特殊情况的时候，即：$\rho_i(x)=x,l_j=-\infin ,u_j=+\infin$的时候，问题就简化成了一个非线性最小二乘问题：
$$
\frac{1}{2}\sum_{i} \left\|f_i\left(x_{i_1}, ... ,x_{i_k}\right)\right\|^2
$$


#### 2. 具体的例子 

##### Hello World!

求解使得函数：
$$
\frac{1}{2}(10 -x)^2
$$
取得最小值时的自变量$x$的值.

**首先**

定义一个`CostFunction` $f(x)=10-x$

```c++
struct CostFunctor{
    template <typename T>
    bool operator()(const T* const x, T* residual ) const {

        residual[0] = T(10.0) - x[0];
        return true;
    }
};
```

把模板函数写成一个结构体，可以方便调用`CostFunctor::operator<T>()` `T=double`。

```c++
    //定义一个Problem对象
	Problem problem;

	//构造一个CostFunction对象，其内部可以方便地完成自动求导
	//模板参数如下：
    //template <typename CostFunctor,
    //          int kNumResiduals,  // Number of residuals, or ceres::DYNAMIC.
    //          int N0,       // Number of parameters in block 0.
    //          int N1 = 0,   // Number of parameters in block 1.
    //          int N2 = 0,   // Number of parameters in block 2.
    //          int N3 = 0,   // Number of parameters in block 3.
    //          int N4 = 0,   // Number of parameters in block 4.
    //          int N5 = 0,   // Number of parameters in block 5.
    //          int N6 = 0,   // Number of parameters in block 6.
    //          int N7 = 0,   // Number of parameters in block 7.
    //          int N8 = 0,   // Number of parameters in block 8.
    //          int N9 = 0>   // Number of parameters in block 9.
    CostFunction* cost_function = new AutoDiffCostFunction<CostFunctor,1,1>(new CostFunctor);

	//方便添加残差块，参数分别为：CostFunction* , LossFunction* , double *x0, double *x1...)
    problem.AddResidualBlock(cost_function,NULL,&x);

    //Run
    Solver::Options options;
    options.linear_solver_type = ceres::DENSE_QR;
    options.minimizer_progress_to_stdout = true;

    Solver::Summary summary;

    ceres::Solve(options,&problem,&summary);

    std::cout<<summary.BriefReport()<<std::endl;
    std::cout<<"x: "<<initial_x<<" -> "<<x<<" \n";
```



##### Derivatives 

基本模板类：

+ `NumericDiffCostFunctor:`
+ `AutoDiffCostFunction`

Advanced:

+ `DynamicAutoDiffCostFunction`
+ `CostFunctionToFunctor`
+ `NumericDiffFunctor`
+ `ConditionedCostFunction`

除非需要自定义`Jacobian`的计算，否则一般使用`AutoDiffCostFunction`或者`NumericDiffCostFunction`即可。

#### 3.  Powell's Function

求`Powell`函数的最小值

$x=[x_1,x_2,x_3,x_4]$
$$
\begin{split}\begin{align}
   f_1(x) &= x_1 + 10x_2 \\
   f_2(x) &= \sqrt{5}  (x_3 - x_4)\\
   f_3(x) &= (x_2 - 2x_3)^2\\
   f_4(x) &= \sqrt{10}  (x_1 - x_4)^2\\
     F(x) &= \left[f_1(x),\ f_2(x),\ f_3(x),\ f_4(x) \right]
\end{align}\end{split}
$$
求自变量$x$使得$F(x)$的残差$\frac{1}{2}||F(x)||^2$ 取得最小值



#### 4.  曲线拟合

数据来源，对函数$y = e^{0.3x+0.1}$随机采样，并添加$\sigma = 0.2$的高斯噪声

```c++
struct ExponentialResidual {
  ExponentialResidual(double x, double y)
      : x_(x), y_(y) {}

  template <typename T>
  bool operator()(const T* const m, const T* const c, T* residual) const {
    residual[0] = T(y_) - exp(m[0] * T(x_) + c[0]);
    return true;
  }

 private:
  // Observations for a sample.
  const double x_;
  const double y_;
};
```

定义误差，并使用自动求导完成雅克比计算。

```c++
double m = 0.0;
double c = 0.0;

Problem problem;
for (int i = 0; i < kNumObservations; ++i) {
  CostFunction* cost_function =
       new AutoDiffCostFunction<ExponentialResidual, 1, 1, 1>(
           new ExponentialResidual(data[2 * i], data[2 * i + 1]));
  problem.AddResidualBlock(cost_function, NULL, &m, &c);
}
```

把每一个观测都添加进问题中，之后直接进行求解即可。

> 注意事项：误差函数里面该用模板用模板，该加`const`加`const`一个都不能少



### 二、 微分

非线性优化中需要计算`Jacobian`，`ceres`中主要有三种方式：

+ 用户手动求导，并利用`CostFunction`来实现
+ `ceres`使用数值微分来进行近似
+ `Autodiff`自动求导



#### 1. 公式符号说明

统一使用`Spivak’s Notation`

函数$f$ 在$x=a$处的值为$f(a)$，一阶导和二阶导为
$$
D_1 g = \frac{\partial}{\partial x}g(x,y) \text{ and }  D_2 g  = \frac{\partial}{\partial y}g(x,y).
$$
对于二元函数：$g(x,y)$
$$
D_1 g = \frac{\partial}{\partial x}g(x,y) \text{ and }  D_2 g  = \frac{\partial}{\partial y}g(x,y).
$$
$g$的`Jacobian`为：
$$
Dg = \begin{bmatrix} D_1g & D_2g \end{bmatrix}
$$
对于更常见的多元函数：$g:\mathbb{R}^n \rightarrow \mathbb{R}^m$ ，$Dg$表示大小为$m\times n$的`Jacobian`矩阵，其中$D_ig$表示对第$i$个自变量的偏导，对应`Jacobian`中第$i$列。



#### 2. 解析式微分

对于曲线拟合问题中，若函数为：
$$
y= \frac{b_1}{(1+e^{b_2-b_3x})^{1/b_4} }
$$
即给定一系列的数据${x_i,y_i}$，求参数$b_1,b_2,b_3,b_4$的最优值。

目标函数定义为：
$$
\begin{split}\begin{align}
E(b_1, b_2, b_3, b_4)
&= \sum_i f^2(b_1, b_2, b_3, b_4 ; x_i, y_i)\\
&= \sum_i \left(\frac{b_1}{(1+e^{b_2-b_3x_i})^{1/b_4}} - y_i\right)^2\\
\end{align}\end{split}
$$
为了求解这个问题，我们需要定义一个`CostFunction`用于计算对于给定一组$x,y$所对应的残差$f$以及其相对于参数$b_1,b_2,b_3,b_4$的导数。

利用数学知识，我们可以得到：
$$
\begin{split}\begin{align}
D_1 f(b_1, b_2, b_3, b_4; x,y) &= \frac{1}{(1+e^{b_2-b_3x})^{1/b_4}}\\
D_2 f(b_1, b_2, b_3, b_4; x,y) &=
\frac{-b_1e^{b_2-b_3x}}{b_4(1+e^{b_2-b_3x})^{1/b_4 + 1}} \\
D_3 f(b_1, b_2, b_3, b_4; x,y) &=
\frac{b_1xe^{b_2-b_3x}}{b_4(1+e^{b_2-b_3x})^{1/b_4 + 1}} \\
D_4 f(b_1, b_2, b_3, b_4; x,y) & = \frac{b_1  \log\left(1+e^{b_2-b_3x}\right) }{b_4^2(1+e^{b_2-b_3x})^{1/b_4}}
\end{align}\end{split}
$$
`CostFunction`的实现如下：

```c++
class Rat43Analytic : public SizedCostFunction<1,4> {
   public:
     Rat43Analytic(const double x, const double y) : x_(x), y_(y) {}
     virtual ~Rat43Analytic() {}
     virtual bool Evaluate(double const* const* parameters,
                           double* residuals,
                           double** jacobians) const {
       const double b1 = parameters[0][0];
       const double b2 = parameters[0][1];
       const double b3 = parameters[0][2];
       const double b4 = parameters[0][3];

       residuals[0] = b1 *  pow(1 + exp(b2 -  b3 * x_), -1.0 / b4) - y_;

       if (!jacobians) return true;
       double* jacobian = jacobians[0];
       if (!jacobian) return true;

       jacobian[0] = pow(1 + exp(b2 - b3 * x_), -1.0 / b4);
       jacobian[1] = -b1 * exp(b2 - b3 * x_) *
                     pow(1 + exp(b2 - b3 * x_), -1.0 / b4 - 1) / b4;
       jacobian[2] = x_ * b1 * exp(b2 - b3 * x_) *
                     pow(1 + exp(b2 - b3 * x_), -1.0 / b4 - 1) / b4;
       jacobian[3] = b1 * log(1 + exp(b2 - b3 * x_)) *
                     pow(1 + exp(b2 - b3 * x_), -1.0 / b4) / (b4 * b4);
       return true;
     }

    private:
     const double x_;
     const double y_;
 };
```

优化较少重复计算：

```c++
class Rat43AnalyticOptimized : public SizedCostFunction<1,4> {
   public:
     Rat43AnalyticOptimized(const double x, const double y) : x_(x), y_(y) {}
     virtual ~Rat43AnalyticOptimized() {}
     virtual bool Evaluate(double const* const* parameters,
                           double* residuals,
                           double** jacobians) const {
       const double b1 = parameters[0][0];
       const double b2 = parameters[0][1];
       const double b3 = parameters[0][2];
       const double b4 = parameters[0][3];

       const double t1 = exp(b2 -  b3 * x_);
       const double t2 = 1 + t1;
       const double t3 = pow(t2, -1.0 / b4);
       residuals[0] = b1 * t3 - y_;

       if (!jacobians) return true;
       double* jacobian = jacobians[0];
       if (!jacobian) return true;

       const double t4 = pow(t2, -1.0 / b4 - 1);
       jacobian[0] = t3;
       jacobian[1] = -b1 * t1 * t4 / b4;
       jacobian[2] = -x_ * jacobian[1];
       jacobian[3] = b1 * log(t2) * t3 / (b4 * b4);
       return true;
     }

   private:
     const double x_;
     const double y_;
 };
```

经过优化之后，运算速度有显著的提升。

##### 适用场景

+ 函数形式较为简单
+ 可以利用`Maple`、`Mathematica` 、`SymPy`获取函数的导数
+ 对性能要求极其高
+ 链式法则爱好者或者喜欢求导



#### 3. 数值微分

导数的基本定义：
$$
Df(x) = \lim_{h\rightarrow0}\frac{f(x+h)-f(x)}{h}
$$
前向差分(Forward Differences)
$$
Df(x)\approx \frac{f(x+h)-f(x)}{h}
$$
当$h$足够小的时候，可以用前向差分来近似导数。

使用的例子如下：

```c++
struct Rat43CostFunctor {
  Rat43CostFunctor(const double x, const double y) : x_(x), y_(y) {}

  bool operator()(const double* parameters, double* residuals) const {
    const double b1 = parameters[0];
    const double b2 = parameters[1];
    const double b3 = parameters[2];
    const double b4 = parameters[3];
    residuals[0] = b1 * pow(1.0 + exp(b2 -  b3 * x_), -1.0 / b4) - y_;
    return true;
  }

  const double x_;
  const double y_;
}

CostFunction* cost_function =
  new NumericDiffCostFunction<Rat43CostFunctor, FORWARD, 1, 4>(
    new Rat43CostFunctor(x, y));
```

实现细节如下：

```c++
class Rat43NumericDiffForward : public SizedCostFunction<1,4> {
   public:
     Rat43NumericDiffForward(const Rat43Functor* functor) : functor_(functor) {}
     virtual ~Rat43NumericDiffForward() {}
     virtual bool Evaluate(double const* const* parameters,
                           double* residuals,
                           double** jacobians) const {
       functor_(parameters[0], residuals);
       if (!jacobians) return true;
       double* jacobian = jacobians[0];
       if (!jacobian) return true;

       const double f = residuals[0];
       double parameters_plus_h[4];
       for (int i = 0; i < 4; ++i) {
         std::copy(parameters, parameters + 4, parameters_plus_h);
         const double kRelativeStepSize = 1e-6;
         const double h = std::abs(parameters[i]) * kRelativeStepSize;
         parameters_plus_h[i] += h;
         double f_plus;
         functor_(parameters_plus_h, &f_plus);
         jacobian[i] = (f_plus - f) / h;
       }
       return true;
     }

   private:
     scoped_ptr<Rat43Functor> functor_;
 };
```

当无法计算导数的表达式或者自动求导无法使用的时候可以考虑数值微分。



#### 4.  自动求导(Automatic Derivatives)

##### 基本Demo

简单的Demo如下：

```c++
struct Rat43CostFunctor {
  Rat43CostFunctor(const double x, const double y) : x_(x), y_(y) {}

  template <typename T>
  bool operator()(const T* parameters, T* residuals) const {
    const T b1 = parameters[0];
    const T b2 = parameters[1];
    const T b3 = parameters[2];
    const T b4 = parameters[3];
    residuals[0] = b1 * pow(1.0 + exp(b2 -  b3 * x_), -1.0 / b4) - y_;
    return true;
  }

  private:
    const double x_;
    const double y_;
};


CostFunction* cost_function =
      new AutoDiffCostFunction<Rat43CostFunctor, 1, 4>(
        new Rat43CostFunctor(x, y));
```

与上面使用数值微分的函数进行对比可以发现，重载函数`()`的区别：

数值微分中为：

```c++
bool operator()(const double* parameters, double* residuals) const;
```

自动求导中为：

```c++
template <typename T> bool operator()(const T* parameters, T* residuals) const;
```

自动求导中函数必须为模板函数，并且`const`一个都不能少。

##### 自动求导的原理：

##### 1. 二元数(Dual Number)

二元数是对实数的拓展，引入无穷小量$\epsilon$，满足$\epsilon^2=0$，应用例子如下：
函数：
$$
f(x)= x^2
$$

$$
f(10+\epsilon) = 100 + 20\epsilon
$$

即：$Df(10)=20$，$f(x+\epsilon)=f(x)+Df(x)\epsilon$ 

##### 2. Jets

`Jet`为$n$维度二元数，由一个实部和$n$个无穷小量$\epsilon_i,i=1,...,n$组成，满足性质：$\forall i,j: \epsilon_i\epsilon_j = 0 $ ，写成向量形式如下：
$$
x = a + \sum_j v_{j} \epsilon_j \\
x = a + \mathbf{v} \\
f(a + \mathbf{v}) = f(a) + Df(a) \mathbf{v}.
$$
对于多元函数$f:\mathbb{R}^n\rightarrow \mathbb{R}^m $ ，其在$x_i = a_i + \mathbf{v}_i,\ \forall i = 1,...,n$处的取值为：
$$
f(x_1,...,x_n) = f(a_1,...,a_n) + \sum_{i}D_if(a_1,...,a_n)\mathbf{V}_i　\\
f(x_1,..., x_n) = f(a_1, ..., a_n) + \sum_i D_i f(a_1, ..., a_n) \epsilon_i
$$

> 此处$Df(a)$为行向量表示

便可以得出`Jacobian`矩阵。

##### 自动求导缺点

自动推导出的函数导数表达式有时候会过于复杂，导致效率变低



#### 5. 使用自动微分接口

问题描述：
$$
\begin{split}\min & \quad \sum_i \left \|y_i - f\left (\|q_{i}\|^2\right) q_i
\right \|^2\\
\text{such that} & \quad q_i = R(\theta) x_i + t\end{split}
$$
二维带有畸变的仿射变换(2D Affine)

```c++
template <typename T> T TemplatedComputeDistortion(const T r2) {
  const double k1 = 0.0082;
  const double k2 = 0.000023;
  return 1.0 + k1 * r2 + k2 * r2 * r2;
}

struct Affine2DWithDistortion {
  Affine2DWithDistortion(const double x_in[2], const double y_in[2]) {
    x[0] = x_in[0];
    x[1] = x_in[1];
    y[0] = y_in[0];
    y[1] = y_in[1];
  }

  template <typename T>
  bool operator()(const T* theta,
                  const T* t,
                  T* residuals) const {
    const T q_0 =  cos(theta[0]) * x[0] - sin(theta[0]) * x[1] + t[0];
    const T q_1 =  sin(theta[0]) * x[0] + cos(theta[0]) * x[1] + t[1];
    const T f = TemplatedComputeDistortion(q_0 * q_0 + q_1 * q_1);
    residuals[0] = y[0] - f * q_0;
    residuals[1] = y[1] - f * q_1;
    return true;
  }

  double x[2];
  double y[2];
};
```

直接利用模板函数完成$f$的定义，下面考虑三种无法在自动求导中使用的$f$定义方法：

1. 非模板函数，直接给出了值
2. 函数计算出值，并可以给出微分
3. 定义成离散数值表的函数，需要完成插值

##### 直接返回值的函数

```c++
double ComputeDistortionValue(double r2);
```

为了在`Affine2DWithDistortion`中使用该函数，我们可以进行如下操作：

1. 把函数`ComputeDistortionValue`包装成一个函数对象`ComputeDistortionValueFunctor`
2. 利用数值微分创建一个`CostFunction`
3. 对`CostFunction`对象使用`CostFunctionToFunctor`进行包装，结果为一个带有模板函数`operator ()`的`Functor`，其内部自动完成`Jacobian`的计算。

实际实现如下：

```c++
struct ComputeDistortionValueFunctor {
  bool operator()(const double* r2, double* value) const {
    *value = ComputeDistortionValue(r2[0]);
    return true;
  }
};

struct Affine2DWithDistortion {
  Affine2DWithDistortion(const double x_in[2], const double y_in[2]) {
    x[0] = x_in[0];
    x[1] = x_in[1];
    y[0] = y_in[0];
    y[1] = y_in[1];

    compute_distortion.reset(new ceres::CostFunctionToFunctor<1, 1>(
         new ceres::NumericDiffCostFunction<ComputeDistortionValueFunctor,
                                            ceres::CENTRAL,
                                            1,
                                            1>(
            new ComputeDistortionValueFunctor)));
  }

  template <typename T>
  bool operator()(const T* theta, const T* t, T* residuals) const {
    const T q_0 = cos(theta[0]) * x[0] - sin(theta[0]) * x[1] + t[0];
    const T q_1 = sin(theta[0]) * x[0] + cos(theta[0]) * x[1] + t[1];
    const T r2 = q_0 * q_0 + q_1 * q_1;
    T f;
    (*compute_distortion)(&r2, &f);
    residuals[0] = y[0] - f * q_0;
    residuals[1] = y[1] - f * q_1;
    return true;
  }

  double x[2];
  double y[2];
  std::unique_ptr<ceres::CostFunctionToFunctor<1, 1> > compute_distortion;
};
```



##### 离散函数

函数$f$为一个映射表，由一系列离散的数值构成：

```c++
vector<double> distortion_values;
```

`ceres`提供了相关插值的函数，方便进行`Cubic`和`Bi-Cubic`插值

使用三次插值的时候需要首先构造一个`Grid1D`对象，把数据进行`Wrap`之后再构建一个`CubicInterpolator`对象去使用它。

代码如下：

```c++
struct Affine2DWithDistortion {
  Affine2DWithDistortion(const double x_in[2],
                         const double y_in[2],
                         const std::vector<double>& distortion_values) {
    x[0] = x_in[0];
    x[1] = x_in[1];
    y[0] = y_in[0];
    y[1] = y_in[1];

    grid.reset(new ceres::Grid1D<double, 1>(
        &distortion_values[0], 0, distortion_values.size()));
    compute_distortion.reset(
        new ceres::CubicInterpolator<ceres::Grid1D<double, 1> >(*grid));
  }

  template <typename T>
  bool operator()(const T* theta,
                  const T* t,
                  T* residuals) const {
    const T q_0 =  cos(theta[0]) * x[0] - sin(theta[0]) * x[1] + t[0];
    const T q_1 =  sin(theta[0]) * x[0] + cos(theta[0]) * x[1] + t[1];
    const T r2 = q_0 * q_0 + q_1 * q_1;
    T f;
    compute_distortion->Evaluate(r2, &f);
    residuals[0] = y[0] - f * q_0;
    residuals[1] = y[1] - f * q_1;
    return true;
  }

  double x[2];
  double y[2];
  std::unique_ptr<ceres::Grid1D<double, 1> > grid;
  std::unique_ptr<ceres::CubicInterpolator<ceres::Grid1D<double, 1> > > compute_distortion;
};
```





### 三、非线性最小二乘问题建模

#### 1. 基本介绍

`ceres`可以看做由两个部分构成，建模`API`负责构建优化问题，之后`solver`可以通过`API`设置最小化算法的相关参数，完成问题的求解。

一般问题可以按照如下形式建模：
$$
\begin{split}\min_{\mathbf{x}} &\quad \frac{1}{2}\sum_{i}
\rho_i\left(\left\|f_i\left(x_{i_1},
... ,x_{i_k}\right)\right\|^2\right)  \\
\text{s.t.} &\quad l_j \le x_j \le u_j\end{split}
$$
表达式：$\rho_i\left(\left\|f_i\left(x_{i_1},...,x_{i_k}\right)\right\|^2\right)$，称为残差块，$f_i(\cdot)$称为`CostFunction`依赖于参数块$\{x_{i1},x_{i2}...x_{ik}\}$ 。

$\rho_i$为`LossFunction`。



#### 2. `CostFunction`

用于计算残差向量与`Jacobian`矩阵，例如：函数$f(x_1,...,x_k)$与参数块$[x_1,...,x_k]$，给定参数$[x_1,...,x_k]$，`CostFunction`会计算函数值以及`Jaocobian`矩阵：
$$
J_i =  \frac{\partial}{\partial x_i} f(x_1, ..., x_k) \quad \forall i \in \{1, \ldots, k\}
$$
`class CostFunction`

```c++
class CostFunction {
 public:
  virtual bool Evaluate(double const* const* parameters,
                        double* residuals,
                        double** jacobians) = 0;
  const vector<int32>& parameter_block_sizes();
  int num_residuals() const;

 protected:
  vector<int32>* mutable_parameter_block_sizes();
  void set_num_residuals(int num_residuals);
};
```

输入输出参数的`size`存放在`CostFunction::parameter_block_sizes_`和`CostFunction::num_residuals`中。

**相关的接口函数**

```c++
bool CostFunction::Evaluate(double const *const *parameters, double *residuals, double **jacobians)
```

参数：

+ `parameters`：二维数组，存储所有的参数，其中`parameters[i]`为大小为`parameter_block_sizes_[i]`的数组，存储着第$i$个参数块的所有参数。
+ `residuals` ：为一个大小为`num_residuals_`的残差数组

`jacobians`：必须注意公式推导中的形式与代码形式的区别。

`jacobians`为一个数组，内部存储的元素为数组，`jacobians`的大小为参数块的个数。

其中，`jabobians[i]`的大小为：`num_residuals x parameter_block_sizes_[i].size()`，其含义为：

每个`residual`对`block[i]`的求导，并且存储在一行，简单的说明如下：
$$
\begin{pmatrix}
y1 \\
y2 \\
y3 \\
\end{pmatrix} = f(block1,block2,block3) \\
J = \begin{pmatrix}
\frac{\partial{y_1}}{\partial{block1}} &\frac{\partial{y_1}}{\partial{block2}} &\frac{\partial{y_1}}{\partial{block3}}\\
\frac{\partial{y_2}}{\partial{block1}} &\frac{\partial{y_2}}{\partial{block2}} &\frac{\partial{y_2}}{\partial{block3}} \\
\frac{\partial{y_3}}{\partial{block1}} &\frac{\partial{y_3}}{\partial{block2}} &\frac{\partial{y_3}}{\partial{block3}} \\
\end{pmatrix}
$$
`jacobian[i]`对应上面的$J$中的一列的元素，内存中存储按照行优先排列。

所以有如下对应关系：
$$
jacobian[i][r * parameter\_block\_sizes\_[i]  + c ] = \frac{\partial{residual[r]}}{\partial{parameters[i][c]}}
$$

`注意`：当`jacobians[i]`为`NULL`的时候，那么参数块`block[i]`会被当做常量，在计算中会被跳过。

#### 3. `SizedCostFunction`

当参数块的大小以及残差向量的大小在编译的时候就已经确定了的话(大多数情况下)，可以使用`SizedCostFunction`，用户只需要实现`CostFunction::Evaluate()`函数即可。

```c++
template<int kNumResiduals,
         int N0 = 0, int N1 = 0, int N2 = 0, int N3 = 0, int N4 = 0,
         int N5 = 0, int N6 = 0, int N7 = 0, int N8 = 0, int N9 = 0>
class SizedCostFunction : public CostFunction {
 public:
  virtual bool Evaluate(double const* const* parameters,
                        double* residuals,
                        double** jacobians) const = 0;
};
```

#### 4. `AutoDiffCostFunction`

由于自定义`CostFunction`以及`SizedCostFunction`很容易出错，所以可以考虑使用`AutoDiffCostFunction`。

```c++
template <typename CostFunctor,
       int kNumResiduals,  // Number of residuals, or ceres::DYNAMIC.
       int N0,       // Number of parameters in block 0.
       int N1 = 0,   // Number of parameters in block 1.
       int N2 = 0,   // Number of parameters in block 2.
       int N3 = 0,   // Number of parameters in block 3.
       int N4 = 0,   // Number of parameters in block 4.
       int N5 = 0,   // Number of parameters in block 5.
       int N6 = 0,   // Number of parameters in block 6.
       int N7 = 0,   // Number of parameters in block 7.
       int N8 = 0,   // Number of parameters in block 8.
       int N9 = 0>   // Number of parameters in block 9.
class AutoDiffCostFunction : public
SizedCostFunction<kNumResiduals, N0, N1, N2, N3, N4, N5, N6, N7, N8, N9> {
 public:
  explicit AutoDiffCostFunction(CostFunctor* functor);
  // Ignore the template parameter kNumResiduals and use
  // num_residuals instead.
  AutoDiffCostFunction(CostFunctor* functor, int num_residuals);
};
```

首先，定义一个重载了`operator()`的`class`或者`struct`，一般称为`functor`。并且必须为模板函数，函数的最后一个参数为计算结果输出的(并且是唯一一个非const参数)。

__例子__：

标量误差：$e=k - x^Ty$，其中$x,y$都是二维向量，$k$为常数，通常误差会加上平方$(k-x^Ty)^2$，但是这些在优化框架内部已经完成了。

对于上述模型代码如下：

```c++
class MyScalarCostFunctor {
  MyScalarCostFunctor(double k): k_(k) {}

  template <typename T>
  bool operator()(const T* const x , const T* const y, T* e) const {
    e[0] = k_ - x[0] * y[0] - x[1] * y[1];
    return true;
  }

 private:
  double k_;
};
```

对于重载函数`operator()`首先输入的是参数`x,y`，输出结果总是在参数的最后一个。上述例子中只有`e[0]`被赋值。

对应的`CostFunction`定义如下：

```
CostFunction* cost_function
    = new AutoDiffCostFunction<MyScalarCostFunctor, 1, 2, 2>(
        new MyScalarCostFunctor(1.0));              ^  ^  ^
                                                    |  |  |
                        Dimension of residual ------+  |  |
                        Dimension of x ----------------+  |
                        Dimension of y -------------------+
```

对于每次观测值，都会有一个`k`在其内部。

`AutoDiffCostFunction`也支持动态决定`CostFunction`中残差的维度，例子如下：

```c++
CostFunction* cost_function
    = new AutoDiffCostFunction<MyScalarCostFunctor, DYNAMIC, 2, 2>(
        new CostFunctorWithDynamicNumResiduals(1.0),   ^     ^  ^
        runtime_number_of_residuals); <----+           |     |  |
                                           |           |     |  |
                                           |           |     |  |
          Actual number of residuals ------+           |     |  |
          Indicate dynamic number of residuals --------+     |  |
          Dimension of x ------------------------------------+  |
          Dimension of y ---------------------------------------+
```

#### 5. `DynamicAutoDiffCostFunction` 

`AutoDiffCostFunction`需要在程序编译的时候就已经知道参数块的数量和大小，并且限制在最多十个参数块，有些时候并不适用。

```c++
template <typename CostFunctor, int Stride = 4>
class DynamicAutoDiffCostFunction : public CostFunction {
};
```

使用例子如下：

```c++
struct MyCostFunctor {
  template<typename T>
  bool operator()(T const* const* parameters, T* residuals) const {
  }
}
```

你需要额外指定参数的大小：

```c++
DynamicAutoDiffCostFunction<MyCostFunctor, 4>* cost_function =
  new DynamicAutoDiffCostFunction<MyCostFunctor, 4>(
    new MyCostFunctor());
cost_function->AddParameterBlock(5);
cost_function->AddParameterBlock(10);
cost_function->SetNumResiduals(21);
```



#### 6. `NumericDiffCostFunction`

略，用到再补充

##### 数值微分与局部参数(LocalParameterization)

当`costfunction`依赖于manifold上的参数但是`functor`却无法在manifold上计算值的时候，数值微分可能会无法进行。因为`ceres`的数值微分是对每个参数单独添加扰动忽略了参数所在的manifold结构，导致添加扰动后，结果不一定还在该manifold上。例如：单位四元数，添加扰动会破坏其范数为1的约束。

解决这个问题需要`NumerticDiccCostFunction`预先知道与参数块相关联的`LocalParameterization`，只在对应参数块的局部`tangent space`上添加扰动。

#### 7. `DynamicNumericDiffCostFunction`

略

#### 8. `CostFunctionToFunctor`

`CostFunctionToFunctor`是个`adapter class`，使得用户可以在`templated functors`内部使用`CostFunction`对象用于自动微分。

eg:

```c++
class IntrinsicProjection : public SizedCostFunction<2, 5, 3> {
  public:
    IntrinsicProjection(const double* observation);
    virtual bool Evaluate(double const* const* parameters,
                          double* residuals,
                          double** jacobians) const;
};
```

上述代码实现了相机的投影模型对应的`CostFunction`，如果需要添加相机外参信息：

```c++
template<typename T>
void RotateAndTranslatePoint(const T* rotation,
                             const T* translation,
                             const T* point,
                             T* result);
```

实现如下：

```c++
struct CameraProjection {
  CameraProjection(double* observation)
  : intrinsic_projection_(new IntrinsicProjection(observation)) {
  }

  template <typename T>
  bool operator()(const T* rotation,
                  const T* translation,
                  const T* intrinsics,
                  const T* point,
                  T* residual) const {
    T transformed_point[3];
    RotateAndTranslatePoint(rotation, translation, point, transformed_point);

    // Note that we call intrinsic_projection_, just like it was
    // any other templated functor.
    return intrinsic_projection_(intrinsics, transformed_point, residual);
  }

 private:
  CostFunctionToFunctor<2,5,3> intrinsic_projection_;
};
```

#### 9. `LossFunction`

降低`outliers`带来的影响，防止被带偏

```c++
class LossFunction {
 public:
  virtual void Evaluate(double s, double out[3]) const = 0;
};
```

输入为标量$s$，输出为：
$$
out = [\rho(s), \rho'(s),\rho''(s)] 
$$
`ceres`内部定义了一些`loss_function`：

`TrivialLoss`：$\rho(s)=s$

`HuberLoss`：$\begin{split}\rho(s) = \begin{cases} s & s \le 1\\ 2 \sqrt{s} - 1 & s > 1 \end{cases}\end{split}$等。



#### 10. `LocalParameterization`

```c++
class LocalParameterization {
 public:
  virtual ~LocalParameterization() {}
  virtual bool Plus(const double* x,
                    const double* delta,
                    double* x_plus_delta) const = 0;
  virtual bool ComputeJacobian(const double* x, double* jacobian) const = 0;
  virtual bool MultiplyByJacobian(const double* x,
                                  const int num_rows,
                                  const double* global_matrix,
                                  double* local_matrix) const;
  virtual int GlobalSize() const = 0;
  virtual int LocalSize() const = 0;
};
```

如果使参数$x$在维度更低的流形上进行优化，会使求解更加高效。

例子：3D球面是一个二维流形，