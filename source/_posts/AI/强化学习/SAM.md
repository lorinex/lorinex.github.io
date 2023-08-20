---
categories:
  - AI
  - 强化学习
---
Paper: Segment Anything in Medical Image Code: [https://github.com/bowang-lab/MedSAM](https://github.com/bowang-lab/MedSAM) 中文简介：[https://zhuanlan.zhihu.com/p/628617549](https://zhuanlan.zhihu.com/p/628617549)

# SAM

SAM基础模型有很强的泛化能力，通过prompt engineering实现

最常见的prompt包括点、bbox（框）、掩模图和文本描述

任务要求收到任意提示符时均需要输出至少一个有效 mask，即使提示符存在歧义。比如衣服上的点可能是想分割衣服，也可能是想区分人体。这两者至少需要输出一个。

## SAM 结构

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aa191eeb-38bd-41b5-919e-b508d2b39c28/Untitled.png)

- 大规模的数据集
    - 设计了一个数据引擎去制造大量的高质量数据
    - 得到数据集 SA-1B，总共包含了 1100 万张高分辨率图片和 11 亿个 mask。
- Image encoder
    - 输入图像输入前被预处理为 1024*1024，Image encoder 采用MAE VIT-H/16，是经典的视觉 Transformer 结构，最后输出（256，64，64）的图像 embedding。
- Prompt encoder
    - 点和框的 embedding 通过位置编码获得
    - Mask 的 embedding 通过卷积操作获得
    - 文本的 embedding 通过 Clip 的 encoder 获得。
- Mask decoder
    - 首先做 prompt 的 self-attention， prompt 到图像 embedding 的 Cross-attention。
    - 然后，右侧 MLP 均为三层，输出维度与图像 embedding channel 相同的向量，左侧 MLP 为 2048 个神经元，主要作用为聚合全局特征。
    - 使用 MLP 更新 token，再做图像 embedding 到 prompt 的 Cross-attention。经过两轮 decode layer 之后，token 再次与图像 embedding 进行 Cross-attention，output token 作为可训练参数在 decoder 前加入到 prompt 中，分别通过两个 MLP，得到 mask 和 mask 的 IOU（Intersection over Union，用于计算两个集合的相交面积与并集面积的比例）。

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a38abddf-69ca-4221-ab3c-7bcbe9460d03/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bb18553e-8006-4f90-b095-aeaeba9ac40e/Untitled.png)

## SAM的分割模式

SAM 支持三种主要的分割模式：全自动分割模式、边界框模式和点模式

全自动分割模式容易产生无用的区域划分，基于点的模式模糊不清且需要多次预测-校正迭代。

相比之下，基于**边界框的模式**可以明确指定感兴趣区域，无需多次尝试和错误即可获得合理的分割结果。

此外，常用的标注方法之一是在放射学中标注最长直径，如固态肿瘤的反应评估标准（RECIST）。基于 RECIST 标注，可以轻松获得目标的边界框提示。因此，我们认为在使用 SAM 进行医学图像分割时，基于边界框的分割模式比全自动分割和基于点的模式具有更广泛的实用价值。

# SAM运用到医学图像的难点

1. 外观上的巨大差异：自然图像和医学图像在颜色、亮度和对比度方面表现出显著差异。由于所使用的成像模式，例如CT扫描、MRI或超声波，医学图像通常具有不同的特征；
2. 目标物体的模糊边界：医学图像经常显示不同组织和器官之间的模糊边界。受过训练的医学专家对解剖结构有必要的了解，并且能够识别出对于仅根据自然图像训练的模型来说可能不明显的细微边界。

# AutoSAM

> 在医学图像分割任务中，通过冻结Self-Attention Mechanism (SAM) 编码器并微调轻量级任务特定预测Head的方法。SAM是一个可prompt的模型，但对于某些应用场景可能不适用，并且精确的prompt设计在多类分割问题上耗时。为了解决这些问题，作者尝试了三种类型的无prompt预测Head，包括ViT、CNN和线性层。其中，对于ViT Head，作者删除了SAM的Mask解码器中的prompt Token，将其命名为AutoSAM，使其可以在修改后通过一个单一的推理为不同的类生成Mask。该研究通过在有限标签数据的医学图像分割数据集上比较这三种预测Head的结果，发现即使只有一个标记的体积数据，微调SAM也能显著提高其在医学图像数据集上的性能。此外，在缺乏标注数据时，AutoSAM和CNN预测Head也表现出比从头开始训练和自监督学习方法更好的分割精度。因此，该微调方法在标签效率方面表现出色，为医学图像分割任务提供了一种有效的解决方案，并消除了对prompt的依赖，扩展了模型的适用性。

代码：[https://link.zhihu.com/?target=https%3A//github.com/xhu248/AutoSAM](https://link.zhihu.com/?target=https%3A//github.com/xhu248/AutoSAM)

论文：[https://arxiv.org/pdf/2306.13731.pdf](https://arxiv.org/pdf/2306.13731.pdf)

介绍：[https://zhuanlan.zhihu.com/p/641018051](https://zhuanlan.zhihu.com/p/641018051)

作者将SAM中的Mask解码器替换为不需要prompt进行训练和推理的预测Head。本文评估了三种不同类型的预测Head，包括视觉Transformer（ViT）、卷积神经网络（CNN）和线性层。ViT预测Head采用SAM Mask解码器，命名为AutoSAM，由轻量级交叉注意力模块和转置卷积层组成。作者移除prompt标记并复制图像嵌入以及其他辅助嵌入，以便解码器可以同时为不同的类生成多个Mask。

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/853a9fbd-b738-49e8-886b-26aa1e0c926b/Untitled.png)

## ****Prediction Head****

作者将预测Head设计为不可prompt的，并且唯一的输入是来自SAM编码器的图像嵌入，并探讨了3种最常见的体系结构类型，ViT、CNN和线性层。

### ****Vision Transformer****

在SAM中，原始的掩膜解码器（mask decoder）采用了Vision Transformer（ViT）作为骨干网络。因此，我们对其进行轻微修改，使得预测头部不仅是不可提示的（non-promptable），还能够利用SAM掩膜解码器中的权重。如图2所示，在SAM解码器中，除了提示标记（prompt token）和图像嵌入（image embedding）之外，还包括可训练的输出标记（output tokens），其中包括生成掩膜的掩膜标记（mask token）和用于预测掩膜置信度的IoU标记（IoU token）。此外，掩膜标记还包括前景掩膜标记（foreground mask token）和背景掩膜标记（background mask token）。输出标记与提示标记进行连接，我们将其称为辅助嵌入（auxiliary embeddings）。

在双向注意力模块中，每个层都执行自注意力和交叉注意力。关于交叉注意力，它包括从标记（作为查询）到图像嵌入和从图像嵌入到标记（作为键和值）的过程。然后，图像嵌入通过两个转置卷积层进行上采样，并选择前景掩膜标记来与上采样的嵌入进行逐点乘积，以获取掩膜。

相比之下，AutoSAM删除了辅助嵌入中的提示标记，因此它不再是一个可提示的模型。另一个修改是通过类别数量的倍数复制辅助嵌入和图像嵌入，以生成多个类别的掩膜。每个类别的计算可以并行进行，因此生成额外掩膜的开销可以忽略不计。还有一种生成单次推理中多个掩膜的方法是简单地在输出标记中添加更多的前景掩膜标记。然而，我们选择了第一种策略，因为直观上，一组辅助嵌入在SAM中代表一个要进行分割的对象。AutoSAM使得可以独立地为每个类别生成掩膜。

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fd409602-e9c7-49f2-abc2-2f1ce0345931/Untitled.png)

作者注意到SAM中的原始Mask解码器具有ViT Backbone，因此作者可以对其进行轻微修改，以便预测Head不仅不可prompt，而且能够利用SAM Mask解码器中的权重。

如图2所示，对于SAM解码器，除了prompt Token 和图像嵌入之外，还有可训练的输出 Token ，包括用于生成Mask的Mask Token 和用于预测Mask置信度的IoU Token 。

此外，Mask Token 包括前景Mask Token 和背景Mask Token 。输出 Token 与prompt Token 连接，作者将其命名为辅助嵌入。在双向注意力模块中，每一层都进行自注意力和交叉注意力。关于交叉注意力，它包括从 Token 到图像嵌入，以及从图像嵌入到 Token （作为密钥和值）。然后，通过2个转置的conv层对图像嵌入进行放大，并选择前景Mask Token 与放大的嵌入进行逐点乘积以获得Mask。

相比之下，AutoSAM删除辅助嵌入中的prompt标记，使其不再是可prompt的模型。另一种修改是通过类的数量复制辅助嵌入和图像嵌入，以生成多个类的Mask。每对的计算可以并行进行，因此与生成额外Mask相关的开销是可以忽略的。为一个推理生成多个Mask的替代方法是简单地在输出 Token 中添加更多前景Mask Token 。然而，作者选择第一种策略是因为，直观地说，一组辅助嵌入表示SAM中要分割的一个目标。AutoSAM独立地为每个类启用生成Mask。

### CNN

"Convolutional Neural Network"（卷积神经网络）是一种在许多流行的医学图像分割模型中用于解码器（decoder）的预测头部表示，例如UNet [23]、UNet++ [31]、TransUNet [5]和Swin-UNetr [9]等。首先，我们将图像嵌入（image embedding）重新塑造成一个大小为（256，64，64）的特征图。接着，根据UNet的结构，CNN头部被分为k个阶段（k >= 2），每个阶段包含一个步长为1的卷积层和一个步长为2的转置卷积层进行上采样。在实验部分会尝试不同的k值，当k > 2时，转置卷积层会被替换为第k-2个阶段的卷积层，以确保输出特征图始终是4倍上采样。最后，应用一个点卷积层，核大小为1，来生成每个类别的预测掩膜。

这种类型的预测Head是许多流行的医学图像分割模型中解码器的表示，如UNet、UNet++、TransUNet和Swin-UNetr。作者首先将嵌入的图像Reshape为大小为（256,64,64）的特征图。根据UNet中的结构，CNN Head部有k个阶段（k>=2），每个阶段由Stride为1的conv层和Stride为2的转置conv层组成。

在实验部分尝试了不同的k值，当k＞2时，在k−2阶段，转置的conv层被替换为conv层，使得输出特征图总是放大4x。最后，应用kernel-size为1的逐点conv层来生成每个类的预测Mask。

### ****Linear Layer****

简单的分类Head总是用于评估在预训练任务中学习的特征表示的泛化。在这项工作中，作者还应用线性Head来测试是否存在SAM编码器提取的高级语义信息。与CNN相同，作者将嵌入的图像重新映射为2D特征图，然后直接部署2个转置conv层。然后，作者使用2个kernel-size为1的conv层来代替MLP来获得每个像素的分类。

## 实验结果

1. Label-efficient Adaptation（标签高效适应）：
    - 在微调模型时，希望在有限标注图像的情况下获得有希望的结果，以降低标注成本。
    - AutoSAM和CNN Head表现出与其他方法相比最好的分割精度，特别是当只有1个标记时，AutoSAM的平均Dice分数几乎是UNet和SimCLR的两倍。
    - 在统计显著性方面，很难确定AutoSAM或CNN Head是否具有更高的Dice分数，但是这也意味着SAM的强大效果主要是由图像编码器而不是Mask解码器提取的特征所致。
    - SAM表现出更差的分割性能，即使只用1个标记训练，这支持了微调SAM是解决其在医学图像数据集上性能下降的有效方法。SAM的较低ASSD得分可能归因于其解码器旨在生成集中在prompt位置附近的目标的Mask。
2. Ablation Study（消融研究）：
    - 对CNN预测Head中的深度数量进行消融研究，结果发现Dice随着深度的增加而增加，但当Depth > 4时，从增加预测Head中的参数所获得的好处开始减少。
    - 在不同的编码器尺寸下（ViT-b、ViT-l和ViT-h）评估AutoSAM和Encoder+CNN的性能，发现较大的模型大小通常会产生更好的微调结果，但AutoSAM对编码器架构的敏感性不如Encoder+CNN。
    - 使用更多标记数据进行微调时，AutoSAM仅在标记的卷数小于10时具有优势，由于SAM在大规模图像数据集上预训练，图像编码器能够提取有利于下游分割任务的语义信息。然而，当标记数据足够时，从自然图像中获得的知识可能会对用于医学图像的预测Head产生负面影响，因此需要大规模的医学图像数据集来预训练SAM。

## 总结

作者探索了三种类型的预测Head，ViT（称为AutoSAM）、CNN和线性层，其中AutoSAM和CNN Head在Few-Shot Head学习设置中显示出有希望的结果。仅用一个标记进行微调比框prompt的SAM具有更好的性能，这一事实证明了为新数据集定制SAM的必要性。由于标注的数量有限，作者的方法优于从Head开始训练和自监督学习基线。

# DeSAM

paper: [https://arxiv.org/pdf/2306.00499](https://arxiv.org/pdf/2306.00499)

code: [https://github.com/yifangao112/DeSAM](https://github.com/yifangao112/DeSAM).

introduction: [https://zhuanlan.zhihu.com/p/642835198](https://zhuanlan.zhihu.com/p/642835198)

**在SAM模型的掩码解码器的交叉注意转换器层中，image embedding和prompt token会相互作用，从而使得最终输出的掩码高度依赖于提示**。

作者将SAM的掩码解码器解耦为两个子任务：与提示相关的IoU回归阶段（prompt-relevant IoU regression）和提示不变的掩码学习阶段（prompt-invariant mask learning）。为此，作者设计了两个对应的模块将其添加到全自动 SAM 中，第一个模块为：**prompt-relevant IoU module (PRIM)**，提示相关的IoU模块，根据给定的提示预测IoU值并生成mask embeddings；第二个模块为：**prompt-invariant mask module (PIMM)，**提示不变掩码模块，它将来自图像编码器的image embeddings与来自 PRIM 的mask embeddings融合以生成mask。DeSAM最大限度地减少了由分割任意模式中的错误提示引起的性能下降。此外，由于不需要训练图像编码器，image embeddings可以在训练前被提前计算，从而显著降低了对GPU内存的需求。基于全尺寸SAM模型（ViT-H）的DeSAM可以在入门级GPU上运行，这比以往基于编码器的微调方法效率高得多。在公开可用的前列腺数据集上进行的大量实验表明，DeSAM提高了全自动前列腺分割任务的鲁棒性。

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ec34516c-5535-4130-9508-c37911887a9a/Untitled.png)

# MedLAM

paper: [https://arxiv.org/pdf/2306.14752](https://arxiv.org/pdf/2306.14752)

code: [https://github.com/openmedlab/MedLSAM](https://github.com/openmedlab/MedLSAM)

introduction: [https://blog.csdn.net/qq_45745941/article/details/131483132](https://blog.csdn.net/qq_45745941/article/details/131483132)

用于三维医学图像定位的基础模型

由两个主要组件组成：相对距离回归（RDR）和多尺度相似性（MSS）

# 3DSAM-adapter

2023-06

paper: [https://arxiv.org/pdf/2306.13465v1.pdf](https://arxiv.org/pdf/2306.13465v1.pdf)

code: [https://github.com/med-air/3DSAM-adapter](https://github.com/med-air/3DSAM-adapter)

# Medical SAM Adapter

paper: [https://arxiv.org/pdf/2304.12620](https://arxiv.org/pdf/2304.12620)

code: [https://github.com/WuJunde/Medical-SAM-Adapter](https://github.com/WuJunde/Medical-SAM-Adapter)

introduction: [https://mp.weixin.qq.com/s/ykEbDt_6lm9muKTDs6evHQ](https://mp.weixin.qq.com/s/ykEbDt_6lm9muKTDs6evHQ)

**为什么选择PEFT和Adaption？**

PEFT已经被证明是一种有效的策略，可以针对特定用途对大型的FM进行微调，与完全微调相比，它保持了大部分参数的冻结，学习的参数量明显减少，通常**少于总数的5%**，这就使得效率更高。研究也表明了，PEFT方法比完全微调更好，因为它能够避免**灾难性遗忘**，也能够更好地推广到**域外场景**，尤其是在**数据量不足的情况下**，在所有的PEFT中，Adaption是一个有效的工具，不仅在NLP中，在CV领域也很有效，Adaption可以很容易被用在各种下游的CV任务上，所以作者认为Adaption是将SAM用到医学领域的最合适的技术。

MSA架构

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/df36f423-7673-4de1-871d-1f39a55198a4/Untitled.png)

本文没有完全调整所有参数，而是保持了预训练的SAM参数，并在架构的特定位置插入Adapter模块。Adapters一个BottleNeck结构，依次使用**下投影、ReLU激活、上投影**，图1*【原文写的都是图2，应该是之前有个什么图被删了，然后文章没改过来】*的（b）展示了一个例子，下投影是**使用一个简单MLP将给定的嵌入压缩到一个比较小的维度**，上投影则使用另一个MLP将压缩后的嵌入扩充到原始维度。

在SAM编码器中，作者为每个ViT块部署了两个Adapter，对于一个标准的ViT块（如图1的（a）所示），作者将第一个Adapter放在**多头注意力之后和残差连接之前**（如图1的（b）所示），第二个Adapter**放在多头注意力之后MLP的残差捷径中**，第二个Adaption之后，作者还使用了一个**缩放系数s对嵌入进行缩放**。

在SAM解码器中，作者为每个ViT块部署了三个Adapter，第一个放在**从prompt到图像嵌入的多头交叉注意力之后，并对prompt嵌入进行了残差相加**，这里作者对Adapter进行了小的修改，使用**另一个下投影来压缩prompt的嵌入**，并在ReLU激活之前将其添加到Adapter的嵌入之中，这就将prompt信息整合到了模块中，有助于Adapter根据prompt信息调整参数，对不同的模态以及下游任务能够更好地适应。第二个的部署方式和编码器完全一致，第三个部署在**图像嵌入向量的残差连接后**，以促进图像嵌入到prompt的交叉注意力。在适配后，另一个残差连接和LayerNorm被连接起来，用来输出最终的结果。值得注意的是，作者只在解码器的两个块的第一个块中部署了Adapter，第二个块和mask预测头完全**基于给定的数据运行**。

将SAM应用于医学图像分割的另一个挑战是**图像维度的不同**，与2D图像不同，许多医疗扫描是3D的，比如MRI和CT，这些3D模态在临床使用中至关重要，尽管SAM可以应用于**体的每个切片以获得最好的分割，但是不考虑深度维度上的相关性**，许多研究表明**深度相关性对于3D医学图像分割至关重要**，为了解决这个问题，提出了一种新的方法，这种方法的灵感来源于图像到视频的Adaption。具体结构如图1的（c）所示，在每个区块中，作者将注意力操作分成两个分支，**空间分支与深度分支**，对于一个给定深度为D的3D样本，作者将**D×N×L发送到空间分支的多头注意力**，其中N的嵌入向量的个数，L是嵌入向量的长度，D是**操作的数量**，交叉注意力运用在N×L维度上以学习与抽象空间的相关性作为嵌入。在深度分支上，首先转置输入矩阵，以获得一个**N×D×L**的输入，然后用相同的结构去处理，但是交叉应用于D×L维度上，通过这种方式，深度相关性被学习到，最后再把深度分支的结果转置回其原始形状，并且添加到空间分支中。

# SAMM

paper: [https://arxiv.org/pdf/2304.05622](https://arxiv.org/pdf/2304.05622)

code: [https://github.com/bingogome/samm](https://github.com/bingogome/samm)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5a278a47-0538-4bbf-b2ba-def04b59b660/Untitled.png)

图1展示了SAMM的整体架构，其中包括一个加载了预训练模型的SAM服务器和用于3DSlicer的交互式提示插件（Slicer-IPP）。首先，Slicer-IPP通过3DSlicer的内置接口处理所有图像切片。然后，它处理所有图像，并随后将图像的嵌入映射到二进制数组格式中，存储在随机访问内存（RAM）中，以实现高效的存储和检索。SAM服务器与Slicer-IPP并行运行，并监视从3DSlicer发送的请求。服务器端托管本地的SAM，它使用嵌入策略将从RAM中获取的图像数据整合到模型中。SAM中的图像编码器使用经过预训练的MAE视觉变换器（ViT）[Dosovitskiy等，2020]来降低输入图像的尺寸，并在运行时同时检测（嵌入）图像特征。Slicer-IPP为最终用户提供提示（通常是放置在图像切片上的点），以添加或删除所选区域。提示点被传输到SAM的提示编码器，随后使用提示和预加载的图像嵌入生成掩码。图像嵌入的过程后面紧随着基于嵌入特征和用户定义提示的分割推理步骤。请注意，图像编码器每个图像运行一次，而不是每个提示运行一次，这允许用户实时使用不同提示多次对同一图像进行分割。鉴于图像嵌入的初始化提前发生，随后的掩码生成过程可以在小延迟内完成（详见第3节）