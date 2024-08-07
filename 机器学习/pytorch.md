# PyTorch

## Torch

torch 包定义了Tensor数据结构和数学运算，还提供一些其他函数

### 序列化（Serialization）

#### 存储和读取张量

`torch.save`和`torch.load()`可以轻松保存张量：

```python
t = torch.tensor([1., 2.])
torch.save(t, 'tensor.pt')
torch.load('tensor.pt')
```

`torch.save()` 和 `torch.load()` 默认使用 Python 的 pickle，因此您还可以将多个张量保存为 Python 对象的一部分，例如元组、列表和字典等。

需要注意的是，如果你保存的张量是视图，则会将原有张量内存保存到文件中，如果你的视图仅仅只是原有张量的很小一部分，则可能会存储大量没必要的数据，你可以使用`clone()`方法使view独立。

```python
>>> large = torch.arange(1, 1000)
>>> small = large[0:5]
>>> torch.save(small.clone(), 'small.pt')  # saves a clone of small
>>> loaded_small = torch.load('small.pt')
>>> loaded_small.storage().size()
5
```

`torch.load()`会首先在CPU上加载，然后移动到保存他们的设备

#### 推理中保存模型

##### 保存`state_dict`

```python
torch.save(model.state_dict(), PATH)

model = TheModelClass(*args, **kwargs)
model.load_state_dict(torch.load(PATH))
model.eval()
```

##### 保存整个模型

```python
torch.save(model, PATH)

# Model class must be defined somewhere
model = torch.load(PATH)
model.eval()
model.to(device)	# 如果需要换设备
```



#### 训练中保存Checkpoint

- 需要保存除了模型参数以外的东西
- 通过字典的方式加载

```python
torch.save({
            'epoch': epoch,
            'model_state_dict': model.state_dict(),
            'optimizer_state_dict': optimizer.state_dict(),
            'loss': loss,
            ...
            }, PATH)

model = TheModelClass(*args, **kwargs)
optimizer = TheOptimizerClass(*args, **kwargs)

checkpoint = torch.load(PATH)
model.load_state_dict(checkpoint['model_state_dict'])
optimizer.load_state_dict(checkpoint['optimizer_state_dict'])
epoch = checkpoint['epoch']
loss = checkpoint['loss']

model.eval()
# - or -
model.train()
```

## Tensors

tensors类似于数组和矩阵，可以使用在GPU或者其他硬件

### 初始化一个张量

#### 直接从数据创建

```python
data = [[1, 2],[3, 4]]
x_data = torch.tensor(data)
```



#### 从numpy数组创建

```python
np_array = np.array(data)
x_np = torch.from_numpy(np_array)
```



#### 从别的tensor创建

```python
x_ones = torch.ones_like(x_data) # retains the properties of x_data
print(f"Ones Tensor: \n {x_ones} \n")

x_rand = torch.rand_like(x_data, dtype=torch.float) # overrides the datatype of x_data
print(f"Random Tensor: \n {x_rand} \n")
```



#### 使用随机值或常数值

```python
shape = (2,3,)
rand_tensor = torch.rand(shape)
ones_tensor = torch.ones(shape)
zeros_tensor = torch.zeros(shape)

print(f"Random Tensor: \n {rand_tensor} \n")
print(f"Ones Tensor: \n {ones_tensor} \n")
print(f"Zeros Tensor: \n {zeros_tensor}")
```





### 反向传播的梯度追踪与计算

`torch.Tensor`类具有一个属性 `requires_grad` 用以表示该`tensor`是否需要计算梯度，如果该属性设置为`True`的话，表示这个张量需要计算梯度，计算梯度的张量会跟踪其在后续的所有运算。

当我们完成计算后需要反向传播（back propagation）计算梯度时，使用 `.backward()` 即可自动计算梯度。当然，对于一些我们不需要一直跟踪记录运算的`tensor`，也可以取消这一操作，尤其是在对模型进行验证的时候，不会对变量再做反向传播，所以自然不需要再进行追踪，从而减少运算。

每当对于`requires_grad` 为`True`的tensor进行一些运算时（除了用户直接赋值、创建等操作），这些操作都会保存在变量的 `grad_fn` 属性中，该属性返回一个操作，即是上一个作用在这个变量上的操作：

如果需要继续往前得到连续的操作，对`grad_fu`使用 `next_functions` 即可获得其上一步的操作（`next_functions` 返回一个多层的tuple，真正的操作记录对象要经过**两层**的`[0]`索引：

```python
x=torch.ones(2,2,requires_grad=True)
y = x+2
z = y*y*3
out = z.mean()

print(out.grad_fn)
print(out.grad_fn.next_functions[0][0])
print(out.grad_fn.next_functions[0][0].next_functions[0][0])
```

#### 取消追踪运算

有时我们并不需要追踪梯度，将`requires_grad`设置为`False`即可，但是由于有时候有些tensor需要在模型训练时计算梯度，在模型验证时不计算梯度，我们不希望直接对tensor的`requires_grad`属性做更改，所以需要更好、更方便的设置方法：

1. 使用`with torch.no_grad()` 语句块，放在这个语句块下的所有tensor**操作**（不影响tensor本身）都不会被跟踪运算
2. 使用`tensor.detach()` 方法获得一个跟原tensor值一样但是不会被记录运算的tensor



## torch.nn

是模型的基本构建块

### 容器

#### `Module`

所有神经网络模块的父类，自定义的网络也要继承`Module`，



##### `addmodule(name, module)`

添加一个子模块到当前模块中，可以使用`name`制定名字



`apply(fn)`

给所有子模块添加参数，通常使用于初始化模型的参数



`parameters()`

返回模块参数的迭代器，通常将这个返回值传递给优化器。

#### torch.nn.Sequential()

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



## torch.optim

优化器，实现各种优化算法

```python
# 普通构建
optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.9)
# 针对每一层的学习率不同
optim.SGD([{'params': model.base.parameters(), 'lr': 1e-2},
        {'params': model.classifier.parameters()}], lr=1e-3, momentum=0.9)
```

1. Weight Decay:

   Weight Decay是一种正则化技术，通常用来防止模型过拟合。通过给模型权重施加惩罚，weight decay可以减少模型的复杂度，从而使模型在未见过的数据上表现得更好。实际上，它通过在损失函数中添加一个L2惩罚项来实现，即对模型的所有参数的平方和进行惩罚。通常，weight_decay的值设置得比较小，比如1e-2, 1e-3, 1e-4等。

2. Momentum:

   Momentum是一个帮助优化算法更快收敛的技术。在标准的梯度下降中，每一次更新仅仅依赖于当前的梯度值，这有时会导致优化过程停滞或者在鞍点处徘徊。引入momentum后，更新步骤不仅依赖于当前的梯度，还考虑了前一步的更新方向，从而为优化过程提供了一种“惯性”，允许优化过程更平滑地通过噪声区域和鞍点。momentum的值通常设在0.9左右，具体可以根据模型训练的实际情况来微调

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

所有的TorchVision Datasets都有两个属性：`transform`去修改数据，`target_transform`去修改标签

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



## torchvision

### Datasets

所有的类都是继承自`torch.utils.data.Dataset`，可以传递进DataLoader

https://pytorch.org/vision/stable/datasets.html



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



#### Resize

`class torchvision.transforms.Resize(size, interpolation=2)`
功能：重置图像分辨率
参数：
size- If size is an int, if height > width, then image will be rescaled to (size * height / width, size)，所以建议size设定为h*w
interpolation- 插值方法选择，默认为`PIL.Image.BILINEAR`



#### RandomCrop

在随机位置裁剪给定图像。如果图像是 torch Tensor，则预计其形状为 […, H, W]，其中 … 表示任意数量的前导维度，但如果使用非恒定填充，则输入预计最多有 2 个前导维度方面



### utils

#### make_grid

make_grid() 函数可用于创建表示网格中多个图像的张量。该实用程序需要 dtype uint8 的单个图像作为输入。

![sphx_glr_plot_visualization_utils_001](pics/sphx_glr_plot_visualization_utils_001.png)



## 面试题

### pytorch如何微调fine tuning？

