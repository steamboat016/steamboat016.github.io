---
title: "CLIP"
published: 2025-02-26
description: "Notes on CLIP — image-text contrastive learning."
tags: [Multimodal, Vision, Notes]
category: Notes
draft: false
---

- 回顾
	- BERT
		- 自监督：预测掩码部分，预测两句话是否来自同一文本
		- 推理：接到分类头
	- GPT
		- 自监督：文字接龙，预测下一个字
		- 推理：prompt告诉要做什么任务
	- Y.LeCun
		- 强化学习：学到蛋糕上的樱桃
		- 监督学习：学到蛋糕上的糖霜
		- 自监督学习：学习蛋糕胚，蛋糕的主体和结构
- 动机
	- 之前的网络模型只能预测固定类别，难以迁移
	- 仿照GPT，训练一个可迁移的视觉模型，连分类头也不需要
- 论文题目：《利用自然语言的监督信号学习一个可迁移的视觉模型》
- 训练的任务：
	- 起初想法：用图片预测文本
		- 不好！太慢了，而且一个图片对应的文本的可能性是非常多的，没有固定答案
	- CLIP：图片-文本配对![](/notes-images/2025-09-29_160750.png)
		- 让配对的文本和图片向量距离尽可能近，让不配对的文本和图片向量距离尽可能远
	- 这里的image encoder用的是ViT架构，\[CLS\]token的输出表示整个图片的信息
	- 这里的text encoder用的是Transformer结构，使用mask attention，最后一个\[EOS\]token的输出表示整个文本的信息
- 模型推理时，将图片输入模型，只需prompt engineering，将想要分类的label转为文本输入模型，即可得到分类相似度![](/notes-images/2025-09-29_161259.png)
- CLIP的优点![](/notes-images/2025-09-29_161531.png)
	- 用文本查询图像：如在一系列监控画面中利用文本查询匹配的图像![](/notes-images/2025-09-29_161550.png)
- 对比学习概述![](/notes-images/2025-09-29_161431.png)
- 注意：一句文本/一个图像输入CLIP模型得到的是一个向量，而不是一组向量
	- 这里CLIP训练时会将对应的一句话/一个图像编码得到的若干个向量进行聚合得到一个向量使二者相似度最大