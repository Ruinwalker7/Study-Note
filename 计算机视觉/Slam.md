# SLAM

SLAM解决的问题：建图和定位

## 初识

**单目相机：**

- 丢失了深度
- 依靠**视差**来进行估算相对尺度
- 单目视觉无法只依靠图像确定这个真实尺度，成为**尺度不变性**

**双目视觉：**

- 精度一般，精度取决于标定和分辨率
- 计算量大



后端优化：主要用来处理SLAM过程中的噪声

回环检测（闭环检测）：主要解决位置估计随时间漂移的过程

度量地图：使用稀疏和稠密进行分类。稀疏地图进行了一定的抽象，并不需要表达所有的物体，例如：我们选择只保留一部分具有代表意义的东西（路标）

拓扑地图：只关注连通性，而不关注如何从A点到B点



## 三维空间刚体旋转

### 旋转矩阵

#### 点、向量和坐标系

外积：$a\times b$ 能够写成矩阵与向量的乘积$a^{\wedge}b $
$$
a\times b=\begin{Vmatrix}e_1&e_2&e_3\\\\a_1&a_2&a_3\\\\b_1&b_2&b_3\end{Vmatrix}=\begin{bmatrix}a_2b_3-a_3b_2\\\\a_3b_1-a_1b_3\\\\a_1b_2-a_2b_1\end{bmatrix}=\begin{bmatrix}0&-a_3&a_2\\\\a_3&0&-a_1\\\\-a_2&a_1&0\end{bmatrix}\boldsymbol{b}\stackrel{\text{def}}{=}\boldsymbol{a}^{\wedge}\boldsymbol{b}.
$$
$a^{\wedge}$也被称为反对称矩阵

#### 坐标系间欧式变换

旋转矩阵的性质：

1. 矩阵$R$描述了旋转本身，而与矩阵具体内容无关；

#### 变换矩阵与齐次坐标

特殊欧式群：
$$
\mathrm{SE}(3)=\left\{\boldsymbol{T}=\begin{bmatrix}\boldsymbol{R}&\boldsymbol{t}\\\boldsymbol{0}^\mathrm{T}&1\end{bmatrix}\in\mathbb{R}^{4\times4}|\boldsymbol{R}\in\mathrm{SO}(3),\boldsymbol{t}\in\mathbb{R}^3\right\}.
$$
与SO(3)一样，求解该矩阵的逆就是求反向的变换：
$$
\boldsymbol{T}^{-1}=\begin{bmatrix}\boldsymbol{R}^\mathrm{T}&-\boldsymbol{R}^\mathrm{T}\boldsymbol{t}\\\boldsymbol{0}^\mathrm{T}&1\end{bmatrix}.
$$

### 旋转向量和欧拉角

#### 旋转向量

考虑某个$R$表示的旋转矩阵，如果用旋转向量来表示，假设旋转轴为一个单位长度的向量$n$，角度为$\Theta$，旋转向量到旋转矩阵的变换由罗德里格斯公式描述：
$$
\boldsymbol{R}=\cos\theta\boldsymbol{I}+\left(1-\cos\theta\right)\boldsymbol{n}\boldsymbol{n}^\mathrm{T}+\sin\theta\boldsymbol{n}^\wedge.
$$
相反我们也可以计算一个反变换：
$$
\begin{aligned}
\operatorname{tr}\left(R\right)& =\cos\theta\operatorname{tr}(\boldsymbol{I})+(1-\cos\theta)\operatorname{tr}\left(\boldsymbol{n}\boldsymbol{n}^\mathrm{T}\right)+\sin\theta\operatorname{tr}(\boldsymbol{n}^\mathrm{\wedge}) \\
&=3\cos\theta+(1-\cos\theta) \\
&=1+2\cos\theta 
\end{aligned}
$$
因此：
$$
\theta=\arccos\frac{\operatorname{tr}(\boldsymbol{R})-1}2.
$$


#### 欧拉角

欧拉角使用**3个分离的转角**，把一个旋转分解成3次绕不同轴的旋转。

欧拉角的定义也非常的多，常见的一个定义是yaw-pitch-roll，他等价于ZYX轴的旋转

欧拉角的重大缺陷**万向锁问题**，当俯仰角为$$\pm90^\circ$$时，第一次旋转和第三次旋转会使用同一个轴，使得系统丢失了一个自由度。理论上可以证明，使用三个实数来表示三维旋转，就不可避免会出现奇异性问题，所以欧拉角不适合插值和迭代。



综上：

1. 旋转矩阵用9个量表示3个自由度的旋转，冗余
2. 欧拉角存在奇异性 

所以我们提出了四元数

### 四元数

在二维情况下，复平面向量旋转可以使用**单位复数**表示，三维旋转可以由**单位四元数**表示

一个四元数 $q$ 由一个实部和三个虚部
$$
q=q_0+q_q\text{i}+q_2\text{j}+q_3\text{k}
$$
三个虚部的运算满足：
$$
\begin{cases}\mathrm{i^2=j^2=k^2=-1}\\\mathrm{ij=k,ji=-k}\\\mathrm{jk=i,kj=-i}\\\mathrm{ki=j,ik=-j}\end{cases}
$$


有时候也使用一个标量和一个向量来表示四元数
$$
q=[s,v]^T,\quad s=q_0\in \mathbb{R},\quad v=[q_1,q_2,q_3]^T\in \mathbb{R}^3
$$
$s$称为实部，$v$称为虚部

#### 四元数的运算

四元数和常见的复数一样，有四则运算，共轭、求逆、数乘等。现在有两个四元数$$q_a,q_b$$

假设两个四元数 $ q_1 $ 和 $ q_2 $ 分别为：

$$ q_1 = a_1 + b_1i + c_1j + d_1k $$

$$ q_2 = a_2 + b_2i + c_2j + d_2k $$

##### 加法
$$ q_1 + q_2 = (a_1 + a_2) + (b_1 + b_2)i + (c_1 + c_2)j + (d_1 + d_2)k $$

##### 减法
$$ q_1 - q_2 = (a_1 - a_2) + (b_1 - b_2)i + (c_1 - c_2)j + (d_1 - d_2)k $$

##### 乘法
$$
 q_1 q_2 = (a_1a_2 - b_1b_2 - c_1c_2 - d_1d_2) + (a_1b_2 + b_1a_2 + c_1d_2 - d_1c_2)i + (a_1c_2 - b_1d_2 + c_1a_2 + d_1b_2)j + (a_1d_2 + b_1c_2 - c_1b_2 + d_1a_2)k 
$$



##### 模
四元数 $ q $ 的模 $ \|q\| $ 为：

$$ \|q\| = \sqrt{a^2 + b^2 + c^2 + d^2} $$

##### 共轭

四元数共轭是把虚部取成相反数
$$
q^*_a=s_a-x_ai-y_aj-z_ak=[s_a,-v_a]^T
$$
四元数共轭与本身相乘，会得到实四元数
$$
q^*q=qq^*=[s^2_a+v^Tv,0]^T
$$

四元数 $ q $ 的共轭 $ \bar{q} $ 为：

$$ \bar{q} = a - bi - cj - dk $$

##### 逆
四元数 $ q $ 的逆 $ q^{-1} $ 为：
$$
q^{-1} = \frac{\bar{q}}{\|q\|^2} = \frac{a - bi - cj - dk}{a^2 + b^2 + c^2 + d^2}
$$
如果 $q$ 是单位四元数，其逆和共轭是一个量



##### 数乘
标量 $ \lambda $ 与四元数 $ q $ 的数乘为：

$$ \lambda q = \lambda (a + bi + cj + dk) = \lambda a + \lambda bi + \lambda cj + \lambda dk $$



#### 用四元数表示旋转

假设一个空间三维点$$p=[x,y,z]\in\mathbb{R}^3$$，以及一个单位四元数$q$指定的旋转，使用四元数表示如下：

首先把三维空间点使用一个虚四元数表示：
$$
p=[0,x,y,z]^T=[0,v]^T
$$
相当于把四元数的3个虚部与空间中的3个轴对于，旋转后$$p'$$可表示为：
$$
p'=qpq^{-1}
$$
$$p'$$的虚部为旋转后坐标。



#### 四元数到其他旋转表示的转换

四元数的乘法也可以写成一种矩阵的乘法：

假设 $q=[s,v]^T$
$$
\boldsymbol{q}^+=\begin{bmatrix}s&-\boldsymbol{v}^\mathrm{T}\\\\\boldsymbol{v}&s\boldsymbol{I}+\boldsymbol{v}^\wedge\end{bmatrix},\quad\boldsymbol{q}^\oplus=\begin{bmatrix}s&-\boldsymbol{v}^\mathrm{T}\\\\\boldsymbol{v}&s\boldsymbol{I}-\boldsymbol{v}^\wedge\end{bmatrix}.
$$
于是四元数乘法可以写成矩阵形式：
$$
q_1^+q_2=\begin{bmatrix}s_1&-\boldsymbol{v}_1^\mathrm{T}\\\\\boldsymbol{v}_1&s_1\boldsymbol{I}+\boldsymbol{v}_1^\wedge\end{bmatrix}\begin{bmatrix}s_2\\\\\boldsymbol{v}_2\end{bmatrix}=\begin{bmatrix}-\boldsymbol{v}_1^\mathrm{T}\boldsymbol{v}_2+s_1s_2\\\\s_1\boldsymbol{v}_2+s_2\boldsymbol{v}_1+\boldsymbol{v}_1^\wedge\boldsymbol{v}_2\end{bmatrix}=\boldsymbol{q}_1\boldsymbol{q}_2.
$$
从**四元数到旋转矩阵**的变换关系：
$$
R=vv^T+s^2I+2s\hat v+(\hat v)^2
$$

$$
\theta=2\arccos s
$$





## 李群和李代数

### 李群和李代数基础

三维矩阵构成**特殊正交群**SO(3)，变换矩阵构成**特殊欧式群**SE(3)
$$
\begin{aligned}
&\text{SO(3)} =\{R\in\mathbb{R}^{3\times3}|RR^{\mathrm{T}}=\boldsymbol{I},\det(\boldsymbol{R})=1\}, \\
&\mathrm{SE}(3) =\left\{\boldsymbol{T}=\begin{bmatrix}\boldsymbol{R}&\boldsymbol{t}\\\\\boldsymbol{0}^\mathrm{T}&1\end{bmatrix}\in\mathbb{R}^{4\times4}|\boldsymbol{R}\in\mathrm{SO}(3),\boldsymbol{t}\in\mathbb{R}^3\right\}. 
\end{aligned}
$$
旋转矩阵和变换矩阵对加法都不封闭，所以他们不是**良定的**

群：有幺元，逆元，对一种算法封闭

**李群**：具有**连续性质的群**，刚体在三维空间的运动是连续的，所以他们都是李群

#### 李代数的引出



# Eigen使用

```cpp
#include <iostream>
using namespace std;
#include <ctime>

// Eigen 核心部分
#include <Eigen/Core>
// 稠密矩阵的代数运算（逆，特征值等）
#include <Eigen/Dense>

using namespace Eigen;

#define MATRIX_SIZE 50

/****************************
* 本程序演示了 Eigen 基本类型的使用
****************************/

int main(int argc, char **argv) {
  // Eigen 中所有向量和矩阵都是Eigen::Matrix，它是一个模板类。它的前三个参数为：数据类型，行，列
  // 声明一个2*3的float矩阵
  Matrix<float, 2, 3> matrix_23;

  // 同时，Eigen 通过 typedef 提供了许多内置类型，不过底层仍是Eigen::Matrix
  // 例如 Vector3d 实质上是 Eigen::Matrix<double, 3, 1>，即三维向量
  Vector3d v_3d;
  // 这是一样的
  Matrix<float, 3, 1> vd_3d;

  // Matrix3d 实质上是 Eigen::Matrix<double, 3, 3>
  Matrix3d matrix_33 = Matrix3d::Zero(); //初始化为零
  // 如果不确定矩阵大小，可以使用动态大小的矩阵
  Matrix<double, Dynamic, Dynamic> matrix_dynamic;
  // 更简单的
  MatrixXd matrix_x;
  // 这种类型还有很多，我们不一一列举

  // 下面是对Eigen阵的操作
  // 输入数据（初始化）
  matrix_23 << 1, 2, 3, 4, 5, 6;
  // 输出
  cout << "matrix 2x3 from 1 to 6: \n" << matrix_23 << endl;

  // 用()访问矩阵中的元素
  cout << "print matrix 2x3: " << endl;
  for (int i = 0; i < 2; i++) {
    for (int j = 0; j < 3; j++) cout << matrix_23(i, j) << "\t";
    cout << endl;
  }

  // 矩阵和向量相乘（实际上仍是矩阵和矩阵）
  v_3d << 3, 2, 1;
  vd_3d << 4, 5, 6;

  // 但是在Eigen里你不能混合两种不同类型的矩阵，像这样是错的
  // Matrix<double, 2, 1> result_wrong_type = matrix_23 * v_3d;
  // 应该显式转换
  Matrix<double, 2, 1> result = matrix_23.cast<double>() * v_3d;
  cout << "[1,2,3;4,5,6]*[3,2,1]=" << result.transpose() << endl;

  Matrix<float, 2, 1> result2 = matrix_23 * vd_3d;
  cout << "[1,2,3;4,5,6]*[4,5,6]: " << result2.transpose() << endl;

  // 同样你不能搞错矩阵的维度
  // 试着取消下面的注释，看看Eigen会报什么错
  // Eigen::Matrix<double, 2, 3> result_wrong_dimension = matrix_23.cast<double>() * v_3d;

  // 一些矩阵运算
  // 四则运算就不演示了，直接用+-*/即可。
  matrix_33 = Matrix3d::Random();      // 随机数矩阵
  cout << "random matrix: \n" << matrix_33 << endl;
  cout << "transpose: \n" << matrix_33.transpose() << endl;      // 转置
  cout << "sum: " << matrix_33.sum() << endl;            // 各元素和
  cout << "trace: " << matrix_33.trace() << endl;          // 迹
  cout << "times 10: \n" << 10 * matrix_33 << endl;               // 数乘
  cout << "inverse: \n" << matrix_33.inverse() << endl;        // 逆
  cout << "det: " << matrix_33.determinant() << endl;    // 行列式

  // 特征值
  // 实对称矩阵可以保证对角化成功
  SelfAdjointEigenSolver<Matrix3d> eigen_solver(matrix_33.transpose() * matrix_33);
  cout << "Eigen values = \n" << eigen_solver.eigenvalues() << endl;
  cout << "Eigen vectors = \n" << eigen_solver.eigenvectors() << endl;

  // 解方程
  // 我们求解 matrix_NN * x = v_Nd 这个方程
  // N的大小在前边的宏里定义，它由随机数生成
  // 直接求逆自然是最直接的，但是求逆运算量大

  Matrix<double, MATRIX_SIZE, MATRIX_SIZE> matrix_NN
      = MatrixXd::Random(MATRIX_SIZE, MATRIX_SIZE);
  matrix_NN = matrix_NN * matrix_NN.transpose();  // 保证半正定
  Matrix<double, MATRIX_SIZE, 1> v_Nd = MatrixXd::Random(MATRIX_SIZE, 1);

  clock_t time_stt = clock(); // 计时
  // 直接求逆
  Matrix<double, MATRIX_SIZE, 1> x = matrix_NN.inverse() * v_Nd;
  cout << "time of normal inverse is "
       << 1000 * (clock() - time_stt) / (double) CLOCKS_PER_SEC << "ms" << endl;
  cout << "x = " << x.transpose() << endl;

  // 通常用矩阵分解来求，例如QR分解，速度会快很多
  time_stt = clock();
  x = matrix_NN.colPivHouseholderQr().solve(v_Nd);
  cout << "time of Qr decomposition is "
       << 1000 * (clock() - time_stt) / (double) CLOCKS_PER_SEC << "ms" << endl;
  cout << "x = " << x.transpose() << endl;

  // 对于正定矩阵，还可以用cholesky分解来解方程
  time_stt = clock();
  x = matrix_NN.ldlt().solve(v_Nd);
  cout << "time of ldlt decomposition is "
       << 1000 * (clock() - time_stt) / (double) CLOCKS_PER_SEC << "ms" << endl;
  cout << "x = " << x.transpose() << endl;

  return 0;
}
```

