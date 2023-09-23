# PyTorch

## Tensors

tensors类似于数组和矩阵，与



## Dataset & DataLoaders

`torch`提供了两个原始的类：

1. `torch.utils.data.DataLoader`：存储样例和对应标签
2. `torch.utils.data.Dataset`：提供一个更好的遍历方法



### 加载数据集

- root：存储位置
- train：训练集or测试集
- download：是否下载
- `transform` and `target_transform` 指定特征和标签转换

```python
import torch
from torch.utils.data import Dataset
from torchvision import datasets
from torchvision.transforms import ToTensor
import matplotlib.pyplot as plt


training_data = datasets.FashionMNIST(
    root="data",
    train=True,
    download=True,
    transform=ToTensor()
)
```



### 创建自己的数据集

一个通用的数据集类必须要实现下面三个函数，`__init__`，`__len__`，`__getitem__`



### DataLoaders加载数据

我们希望能够小批量的加载数据集，所以需要使用`DataLoader`

```python
from torch.utils.data import DataLoader

train_dataloader = DataLoader(training_data, batch_size=64, shuffle=True)
test_dataloader = DataLoader(test_data, batch_size=64, shuffle=True)

train_features, train_labels = next(iter(train_dataloader))
```







## Torch.Utils.TensorBroad

运行tensorbroad：

```python
pip install tensorboard
tensorboard --logdir=runs
```



`SummeryWriter()`是主要的入口去记录数据



`writer.addImage()`添加一个图像



`writer.add_scalar`

```python
for n_iter in range(100):
    writer.add_scalar('Loss/train', np.random.random(), n_iter)
    writer.add_scalar('Loss/test', np.random.random(), n_iter)
    writer.add_scalar('Accuracy/train', np.random.random(), n_iter)
    writer.add_scalar('Accuracy/test', np.random.random(), n_iter)
```

<img src="pics/hier_tags.png" alt="hier_tags" style="zoom:80%;" />







### Transforms

所有的TorchVision Datasets都有两个属性：`transform`去修改数据，`target_transform`去修改标签



输入

```python
PIL.open()
cv2.read()
```



### torch.stack

将多个相同形状的张量按顺序拼接在一起，形成一个更高维度的张量

```
torch.stack((K, K + 1, K + 2), 0)
```



### torch.normal()

```python
torch.normal(means, std, out=None)
```

返回一个张量，包含从给定参数`means`,`std`的离散正态分布中抽取随机数。 均值`means`是一个张量，包含每个输出元素相关的正态分布的均值.`std`是一个张量，包含每个输出元素相关的正态分布的标准差。 均值和标准差的形状不须匹配，但每个张量的元素个数须相同。



### torch.matmul()

张量乘法，如果维数不统一，就会将低维的扩展



### torch.nn.Sequential()

当一个模型较简单的时候，我们可以使用torch.nn.Sequential类来实现简单的顺序连接模型。

```python
import torch.nn as nn
from collections import OrderedDict
model = nn.Sequential(OrderedDict([
                  ('conv1', nn.Conv2d(1,20,5)),
                  ('relu1', nn.ReLU()),
                  ('conv2', nn.Conv2d(20,64,5)),
                  ('relu2', nn.ReLU())
                ]))
 
print(model)
print(model[2]) # 通过索引获取第几个层
'''运行结果为：
Sequential(
  (conv1): Conv2d(1, 20, kernel_size=(5, 5), stride=(1, 1))
  (relu1): ReLU()
  (conv2): Conv2d(20, 64, kernel_size=(5, 5), stride=(1, 1))
  (relu2): ReLU())
Conv2d(20, 64, kernel_size=(5, 5), stride=(1, 1))
'''
```



### torch.nn.train()和torch.nn.eval()

在评估模型的适合一定要打开torch.nn.eval()，这样会固定住BN和Dropout，不然如果test输入的batch size太小会影响BN和Dropout



### with torch.no_grad

在该模块下，所有计算得出的tensor的requires_grad都自动设置为False。

测试的适合不计算导数，可以节约大量资源



## torchvision

### transforms

#### Compose

将多个变换组合在一起。此转换不支持 torchscript。

```python
transforms.Compose([
    transforms.CenterCrop(10),
    transforms.PILToTensor(),
    transforms.ConvertImageDtype(torch.float),
])
```



#### ToTensor

将一个PIL图像或者一个ndarray从[0,255]转换为[0,1]

在其他情况下，返回张量而不进行缩放。



#### Normalize

`Normalize(mean, std, inplace=False)`

标准化一个`tensor`使用平均值和标准差





### utils

#### make_grid

make_grid() 函数可用于创建表示网格中多个图像的张量。该实用程序需要 dtype uint8 的单个图像作为输入。

![sphx_glr_plot_visualization_utils_001](pics/sphx_glr_plot_visualization_utils_001.png)



