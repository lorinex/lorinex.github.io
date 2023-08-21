---
title: PEFT  
date: 2023-08-21 10:09:43  
tags: [AI,大模型]  
---
# parameter-efficient fine-tuning

[Transformer模型详解（图解最完整版） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/338817680)

PEFT 是基于**预训练大模型**训练出**垂直领域模型**的一类方法，基本的做法就是在预训练大模型的基础上，利用特定领域的数据微调模型，得到在特定领域性能更优的模型。

对于相较大模型参数量不是很大的模型，或者叫常规尺度模型，也存在将模型学习到的知识从一个领域迁移到另一个领域，使得模型在目标域表现更好的一类技术，也就是**迁移学习** (TL, transfer learning)。

大模型因其参数量过于庞大，全参数量的fine-tuning会有如下**问题**：
1. 计算成本过高，对于一般的计算框架，训练一个神经网络所需的GPU内存是模型本身权重大小的10 ~ 12倍 （保存梯度、以及梯度的一阶、二阶矩等信息），以13B LLaMA (FP16) 为例，模型本身大小约24GB，全量重训练则需要240GB以上；
2. 对于有多个垂直领域的场景，全量的fine-tuning会要求每个垂直领域都维护一个全参数量的模型，不同任务间没有共享的参数，比较低效；

## Adapters

Adapters 是最流行的PEFT 之一，基本的做法是在Transformer 中的基础模块：Multi-Head Attention 和 Fully-Connected Feed Forward 之后添加adapter 模块，在fine-tuning的时候，更新adapter模块中的参数，而保持Multi-Head Attention 和 Fully-Connected Feed Forward 中的参数固定不变，如下图所示：

![](source/_posts/AI/SAM/PEFT.assets/image-20230821150946351.png)
Adapter由两个线性层以及残差连接组成，两个线性层中间有一个非线性激活层。为了控制adapter的参数量，一般会把adapter线性层设计成Down-Up的结构，即先对特征降维，然后再进行升维。

Adapter 的数学表达式可以表示为： 
$h_{adapter} = h + (ReLU(hW_{a1} + b_{a1})W_{a2} + b_{a2})$ , 其中 $h$ 为adapter模块的输入， 
$h \in \mathbb{R}^{n \times d_h} W_{a1} \in \mathbb{R}^{d_h \times d_a}, b_{a1} \in \mathbb{R}^{d_a}, W_{a2} \in \mathbb{R}^{d_a \times d_h}, b_{a2} \in \mathbb{R}^{d_h}$, 且 $d_a << d_h$ 。
一般情况，Adapter模块新增的参数量为预训练模型参数量的 0.5\% - 8\% 。

