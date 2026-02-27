---
title: "ViT"
published: 2025-02-26
description: "Notes on Vision Transformer (ViT)."
tags: [Vision, Transformer, Notes]
category: Notes
draft: false
---

## 模型架构
- 切分为若干patch，展平后通过共享的线性投射层转化为embedding（类似于NLP中文本分词通过tokenizer转化为embedding），加上可学习的位置编码传入Transoformer的Encoder部分，另外在最前面还会添加一个用于分类的可学习的\[cls\]token，最终用这个token接一个MLP分类头做分类任务![](/notes-images/2025-09-29_131734.png)
- 这里图像转化为embedding的做法：两种方式![](/notes-images/2025-09-29_132618.png)
- 关于位置编码：不加位置编码/1维位置编码/2维位置编码/相对位置编码（按理来说，2维位置编码应当比较合适）![](/notes-images/2025-09-29_132920.png)
	- 另外作者还做了实验，说明1维位置编码经过训练是可以学到2维信息的（上下左右相邻的patch位置编码也相近）
- 多种模型规格的命名![](/notes-images/2025-09-29_132433.png)
## 对比实验
- 相比于卷积神经网络随着层数增加感受野扩大，ViT在较浅的层就可以关注远距离的全局信息，但是仍然在更深的层更关注远距离的全局信息
- 与卷积神经网络ResNet对比
	- 数据集较小时CNN表现好，随着数据集增大ViT表现好
	- 原因：
		- 归纳偏置![](/notes-images/2025-09-29_135136.png)
- 与混合模型（将卷积神经网络的输出代替patch来输入）对比
	- 混合模型起初表现更好，随着训练代价增大，反而是单纯的ViT表现更好，证明ViT完全可以替代CNN
- 做分类任务的方法
	1. 用上面所说的\[cls\]token，经过Encoder后用该token的输出做分类任务
	2. 不增加token，用最后Encoder所有token输出的全局平均池化(GAP)提取全局信息
	- 二者效果差异不大
- 此外，还做了自监督学习的尝试
	- mask掉一部分patch的embedding，用剩余的可见的patch预测这些mask掉的patch是什么（注意这里等价于要做生成任务，所以还要加Decoder部分），这样训练完成后，可以做迁移（用自监督学习训练完成后的Encoder层的参数作为初始参数做微调即可），效果也很好