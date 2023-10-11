# GAN

GAN包含有两个模型，一个是生成模型（generative model），一个是判别模型(discriminative model)。生成模型的任务是生成看起来自然真实的、和原始数据相似的实例。判别模型的任务是判断给定的实例看起来是自然真实的还是人为伪造的（真实实例来源于数据集，伪造实例来源于生成模型）。

![生成对抗网络（GAN）](assets/v2-d36c35e3bb9ba1aac119304b225c0cda_1440w.image)



G: 生成模型  D: 判别模型

G网络的loss是$$log(1-D(G(z)))$$，而D的loss是$-(log(D(x))+log(1-D(G(z))))$