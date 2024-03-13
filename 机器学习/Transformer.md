# Transformer模型

意义：长距离依赖关系处理和并行计算

https://jalammar.github.io/illustrated-transformer/

## 注意力机制（Attention）

### 为什么我们需要Attention?

希望能够找到重要的东西，从关注全部到关注重点。

简单来说：注意力机制描述了序列元素的加权平均值（带权求和！！！），其权重是根据输入的query和元素的键值进行动态计算的

### Attention原理

在原始的RNN中，Encoder 循环接收上一层的输出以及新的输入进行运算，得到最后的 **hidden states**，就是我们的上下文，将其放入 Decoder 中解码，但是这里存在的问题是处理长难句变得有挑战性，因为当上下文较长的时候，远距离的 hidden states 可能在运算中被忽视了。

Sequence2Sequence 的 Encoder-Decoder 中，所有的 Encoder 层输出的 hidden states 都会传给 Decoder。

Attention Decoder 在产生输出之前执行额外的步骤。为了更加关注对应位置的输入，Decoder 执行以下操作：

1. 查看它收到的 Endcoder hidden states - 每个编码器隐藏状态与输入句子中的某个单词最相关
2. 给每个隐藏状态一个分数
3. 将每个隐藏状态乘以其 softmax 分数，从而放大高分 hidden states，并忽略低分 hidden states

推荐可视化博客：https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/

<video src="../../../Downloads/attention_tensor_dance.mp4"></video>

#### 计算隐藏状态分数

**Query**（**查询**）：Query是一个特征向量，描述我们在序列中寻找什么，即我们可能想要注意什么。

**Keys：**每个输入元素有一个**键**，它也是一个特征向量。该特征向量粗略地描述了该元素“提供”什么，或者它何时可能很重要。键的设计应该使得我们可以根据Query来识别我们想要关注的元素。例如，键通常由编码器产生，代表输入序列中每个词或短语的抽象表示。

**Values：**每个输入元素，我们还有一个**值**向量。这个向量就是我们想要平均的向量。它与键一起被编码，但用途不同。

**Score function：**评分函数，为了对想要关注的元素进行评分，我们需要指定一个评分函数`f`该函数将查询和键作为输入，并输出查询-键对的得分/注意力权重。它通常通过简单的[相似性度量](https://www.zhihu.com/search?q=相似性度量&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A3254012065})来实现，例如[点积](https://www.zhihu.com/search?q=点积&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A3254012065})或MLP。
$$
\alpha_i=\frac{\exp \left(f_{\text {attn }}\left(\text { key }_i, \text { query }\right)\right)}{\sum_j \exp \left(f_{\text {attn }}\left(\text { key }_j, \text { query }\right)\right)}, \quad \text { out }=\sum_i \alpha_i \cdot \text { value }_i
$$
![img](https://pic1.zhimg.com/80/v2-5da9123d899c768f8dacad601c8872c3_720w.webp?source=2c26e567)

**如何生成**：在实际的深度学习模型中，特别是在使用Transformer架构的模型中，key、value和query通常通过对输入数据的不同表示进行线性变换得到。具体来说，对于一个给定的输入向量，模型会使用不同的权重矩阵分别生成key、value和query。这些权重矩阵是模型训练过程中学习得到的，能够将输入数据映射到最适合当前任务的key、value和query表示上。

**匹配过程**：一旦key、value和query被确定，模型将使用一个函数（通常是点积或者某种形式的加权和）来计算每个query与所有key之间的匹配程度。这个匹配程度通常通过softmax函数转换为权重，表示每个value对于回答query的重要性。最后，根据这些权重，模型将所有的value进行加权求和，得到最终的输出。

## 自注意力机制

**自注意力（self-attention）**。在自注意力中，每个序列元素提供一个键、值和query。对于每个元素，根据其query作用一个注意力神经层，检查所有序列元素键的相似性，并为每个元素返回一个不同的平均值向量。

自注意力主要关注于输入序列内部的元素之间的相互作用和依赖关系。这种机制允许模型在处理每个元素时考虑到序列中的所有其他元素，从而捕获序列内部的复杂关系和结构。

### 工作原理

自注意力机制通过计算序列中每个元素对于序列中其他所有元素的注意力分数来工作。这些注意力分数表明了在给定任务（如序列建模、分类等）中，序列的每个部分对于其他部分的重要性程度。自注意力机制的关键优势在于它的自适应性，能够动态地调整序列内部元素间的相互作用强度。

在自注意力机制中，每个元素的表示都会被更新，更新后的表示是原始表示与序列中其他元素的加权和的结合。权重由元素之间的相互作用决定，这样，每个元素的新表示都包含了关于整个序列的全局信息。

### 自注意力的计算步骤

自注意力机制的计算通常包括以下几个步骤：

1. **Query、Key、Value的生成**：对于序列中的每个元素（已经被嵌入到向量中），模型使用不同的权重矩阵生成对应的Query（查询）、Key（键）和Value（值）。这一步骤与前面提到的注意力机制中的生成方式相似。

   - 这些新向量的维度比嵌入向量要小。它们的维度为64，而嵌入和编码器输入/输出向量的维度为512。它们不一定要更小，这是一种架构选择，目的是使多头注意力的计算（大多数情况下）保持恒定。

2. **注意力分数的计算**：模型计算Query与Key的匹配程度，通常通过点积操作实现。这个匹配程度反映了序列中每个元素对其他元素的重要性。

   - 假设我们在处理位置1的单词的自注意力，第一个得分将是q1和k1的点积。第二个得分将是q1和k2的点积。

     ![img](https://jalammar.github.io/images/t/transformer_self_attention_score.png)

3. **归一化和权重的应用**：先将注意力分数除以8（论文中关键向量维度64的平方根 。这样可以得到更稳定的梯度。这里可能存在其他可能的值，但这是默认值）；使用softmax函数对注意力分数进行归一化，将其转换为概率分布形式。然后，根据这些归一化的注意力权重，对Value进行加权求和，得到每个元素的更新表示。

4. **输出表示的合成**：最后，将加权求和得到的表示作为模型的输出，用于后续的处理或任务。

### 自注意力的优势

自注意力机制的一个主要优势是其能够捕获长距离依赖关系。在传统的序列处理模型（如RNN和LSTM）中，处理长序列时往往会遇到性能下降的问题，因为模型难以维持序列开始和结束部分之间的信息流动。自注意力通过直接计算序列内各元素间的关系，有效解决了这一问题，无论元素之间的距离有多远。



## 编码

一般情况下，在自然语言处理应用中，我们会先使用嵌入算法将每个输入词转换为向量。

在Transformer中，每个词都嵌入到一个大小为512的向量中，这发生在最底层的编码器中





可视化，里面讲述词嵌入、transformer的动画很好：https://github.com/bbycroft/llm-viz