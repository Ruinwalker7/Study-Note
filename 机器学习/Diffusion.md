# Diffusion

## 前置知识

### 高斯分布

高斯分布的图形呈现为钟形曲线，称为“钟形曲线”或“正态曲线”。其特征包括：

- **对称性**：高斯分布关于均值 (\mu) 对称。
- **峰度**：均值处有最高的概率密度。
- **范围**：数据点可能出现在整个实数轴上，但大多数点集中在均值的两个标准差范围内。

$$
f(x) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{(x - \mu)^2}{2\sigma^2}}
$$

![高斯分布](https://upload.wikimedia.org/wikipedia/commons/8/8c/Standard_deviation_diagram.svg)

#### KL散度

Kullback-Leibler (KL) 散度是一种衡量两个概率分布之间差异的非对称度量。对于两个高斯分布 $P$ 和 $Q$，其概率密度函数分别为：

$$ P(x) = \mathcal{N}(\mu_1, \sigma_1^2) $$
$$ Q(x) = \mathcal{N}(\mu_2, \sigma_2^2) $$

其KL散度 $D_{KL}(P \| Q)$ 定义为：

$$ D_{KL}(P \| Q) = \int_{-\infty}^{\infty} p(x) \log \left( \frac{p(x)}{q(x)} \right) dx $$

对于两个一维高斯分布，KL散度有一个解析解，可以表示为：

$$ D_{KL}(\mathcal{N}(\mu_1, \sigma_1^2) \| \mathcal{N}(\mu_2, \sigma_2^2)) = \log\left(\frac{\sigma_2}{\sigma_1}\right) + \frac{\sigma_1^2 + (\mu_1 - \mu_2)^2}{2\sigma_2^2} - \frac{1}{2} $$

其中：
- $\mu_1$ 和 $\mu_2$ 分别是分布 $P$ 和 $Q$ 的均值。
- $\sigma_1$ 和 $\sigma_2$ 分别是分布 $P$ 和 $Q$ 的标准差。

KL散度的几个重要性质包括：
- **非负性**：$D_{KL}(P \| Q) \geq 0$，且当且仅当 $P = Q$ 时，KL散度为0。
- **非对称性**：$D_{KL}(P \| Q) \neq D_{KL}(Q \| P)$。



### VAE 变分自编码器

在隐空间中进行采样，通过解码器生成新的图片

### 模型对比

与Gan对比

![img](https://pic2.zhimg.com/80/v2-353a3e644f18fb96731b7ce2cd930505_720w.webp)



### 数学对比



Diffusion模型的训练可以分为两个部分：

1. 前向扩散过程（Forward Diffusion Process） 图片中添加噪声
2. 反向扩散过程（Reverse Diffusion Process） 去除图片中的噪声



![img](https://pic4.zhimg.com/80/v2-3692036352c318ce3bb535b609b21717_720w.webp)

![img](https://pic2.zhimg.com/80/v2-354dd50b9a85531571cb780c1f3c9a41_720w.webp)



不断往里面添加高斯噪声



## 推导