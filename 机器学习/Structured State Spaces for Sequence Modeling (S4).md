# Structured State Spaces for Sequence Modeling (S4)

## Introduce

Transformer能够在中等长度的序列中取得较好效果，但是**无法处理超长序列**，在NLP之外有很多从原始数据中采集到的超长序列，它们的特点往往是特别长且平滑，比如音频、生物识别信号、图像等。

![img](assets/timeseries.png)

所有就要求模型能够解决超长序列中的几个问题：

1. 长距离信息
2. 对于数据的分辨率不敏感
   - 从200MHz的音频或者是100MHz的音频中都能识别出你的声音
3. 在训练和推理的时候都特别有效



![img](assets/s4.png)

S4 使用以下一些内容进行长序列建模：

1. HiPPO
2. Linear State Space Layer
3. Structured State Spaces

## Related Models

![img](assets/paradigms.png)

## 参考

S4博客：https://hazyresearch.stanford.edu/blog/2022-01-14-s4-1

S4的JAX实现：https://srush.github.io/annotated-s4/

