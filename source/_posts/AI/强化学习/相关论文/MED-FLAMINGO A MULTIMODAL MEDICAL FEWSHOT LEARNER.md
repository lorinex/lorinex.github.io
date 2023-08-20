---
categories:
  - AI
  - 强化学习
  - 相关论文
---
VLM：https://zhuanlan.zhihu.com/p/623877502

Med-Flamingo是针对医学领域的**多模态少样本学习模型**，基于OpenFlamingo-9B。它能够进行生成式医学视觉问答，而且在医生评估中的性能提升高达20％，同时还能实现多模态医学少样本适应。

## Introduction
Med-Flamingo is a vision-language model based on Flamingo (Alayrac et al., 2022) that can naturally ingest data with interleaved modalities (images and text), to generate text conditioned on this multimodal input.

Med-Flamingo会生成开放式回答
临床专家给出分数作为评价标准，并创造了一个数据集

本论文有四大贡献，一是提出了multimodal few-shot learner adapted to the medical domain；二是创建了一个数据集，为通用医学领域的多模态少样本学习器提供了预训练数据；三是USMLE-style evaluation dataset，将医学视觉问答与复杂的跨专业医学推理相结合；四是引入医学专家对结果进行评估。

## Related Works

