---
title: "ALBEF"
published: 2025-02-26
description: "Notes on ALBEF — align before fuse with momentum distillation."
tags: [Multimodal, Vision, Language, Notes]
category: multimodal
draft: false
---

- 论文标题：《在融合前对齐：用动量蒸馏的方法进行视觉和语言的表示学习》
- 模型架构![](/notes-images/2025-09-29_211300.png)
	- 这里前面的对齐部分与CLIP几乎相同，不同的是text encoder部分ALBEF用的是双向注意力，最终用\[CLS\]token的输出代表整个文本的信息
	- 这里的cross attention，query来自text encoder，key和value来自image encoder
	- 三个任务：ITC、ITM、MLM
		- ITC(Image-Text Constrastive Learning)：同CLIP
		- ITM(Image-Text Matching)：图文匹配任务，注意这里选择的负例是难负例，即在ITC中相似度最大的负例
		- MLM(Masked Language Model)：结合文本图片，遮住文本的一部分预测遮住的部分
	- 这里动量模型和MOCO类似，但是还加入了动量蒸馏，也就是在计算各个任务的损失时还要加上KL散度，用来评估训练的模型和动量模型的差异
		- 为什么这样做？因为来源于网络的图文对数据集本身其实并不一定完全准确，甚至有时模型的预测比原数据更好，这时再根据原数据更新那就反而会损失模型的性能（误将正例看成是负例），因此要动量蒸馏（学生模型学习教师模型生成的概率分布，教师是动量模型，学生是训练的模型），缓慢更新参数