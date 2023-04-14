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

