# PyTorch

### Dataset加载数据



### TensorBroad

运行tensorbroad：`tensorbroad --logdir logs`

`SummeryWriter()`

`writer.addImage()`



### Transforms

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
