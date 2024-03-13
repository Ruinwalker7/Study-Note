# RLHF

基于人类反馈的强化学习 (RLHF, Reinforcement Learning from Human Feedback) 

在进行预训练模型的时候，模型通常以**预测下一个单词的方式和简单的损失函数** (如交叉熵) 来建模，没有显式地引入人的偏好和主观意见。

 RLHF 的思想：**使用强化学习的方式直接优化带有人类反馈的语言模型**。



## RLHF 技术分解

RLHF 是一项涉及多个模型和不同训练阶段的复杂概念，这里我们按三个步骤分解：

1. 预训练一个语言模型 (LM) ；
2. 聚合问答数据并训练一个奖励模型 (Reward Model，RM) ；
3. 用强化学习 (RL) 方式微调 LM。



### Step1. 预训练一个语言模型 (LM) 

使用经典的预训练目标训练一个语言模型，可选择是否需要进行微调

接下来，我们会基于 LM 来生成训练 **奖励模型** (RM，也叫偏好模型) 的数据，并在这一步引入人类的偏好信息。



### Step 2. 训练奖励模型

**奖励模型** (RM）的训练是 RLHF 区别于旧范式的开端。

奖励模型接收一系列文本并返回一个标量奖励，数值上对应人的偏好。

关于模型选择方面，RM 可以是另一个经过微调的 LM，也可以是根据偏好数据从头开始训练的 LM。例如 Anthropic 提出了一种特殊的预训练方式，即用偏好模型预训练 (Preference Model Pretraining，PMP) 来替换一般预训练后的微调过程。因为前者被认为对样本数据的利用率更高。但对于哪种 RM 更好尚无定论。

关于训练文本方面，RM 的提示 - 生成对文本是从预定义数据集中采样生成的，并用初始的 LM 给这些提示生成文本。Anthropic 的数据主要是通过 Amazon Mechanical Turk 上的聊天工具生成的，并在 Hub 上 [可用](https://huggingface.co/datasets/Anthropic/hh-rlhf)，而 OpenAI 使用了用户提交给 GPT API 的 prompt。

关于训练奖励数值方面，这里需要人工对 LM 生成的回答进行排名。起初我们可能会认为应该直接对文本标注分数来训练 RM，但是由于标注者的价值观不同导致这些分数未经过校准并且充满噪音。通过排名可以比较多个模型的输出并构建更好的规范数据集。

对具体的排名方式，一种成功的方式是对不同 LM 在相同提示下的输出进行比较，然后使用 [Elo](https://en.wikipedia.org/wiki/Elo_rating_system) 系统建立一个完整的排名。这些不同的排名结果将被归一化为用于训练的标量奖励值。

### Step 3. 用强化学习微调

可行方案是使用策略梯度强化学习 (Po**licy Gradient RL) 算法、近端策略优化 (Proximal Policy Optimization，PPO) 微调初始 LM 的**部分或全部参数。

事实证明，RLHF 的许多核心 RL 进步一直在弄清楚如何将熟悉的 RL 算法应用到更新如此大的模型。





待学习：

https://huyenchip.com/2023/05/02/rlhf.html

https://huggingface.co/blog/zh/rlhf

https://www.ibm.com/topics/rlhf

https://en.wikipedia.org/wiki/Reinforcement_learning_from_human_feedback

https://zhuanlan.zhihu.com/p/591474085

https://aws.amazon.com/cn/what-is/reinforcement-learning-from-human-feedback/

https://github.com/allenai/RL4LMs

https://github.com/huggingface/trl

https://arxiv.org/abs/1909.08593

https://en.wikipedia.org/wiki/BLEU

https://en.wikipedia.org/wiki/ROUGE_(metric)