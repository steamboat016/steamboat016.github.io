---
title: "BLIP-2"
published: 2025-02-26
description: "Notes on BLIP-2 — pre-training with frozen image encoder and LLM."
tags: [Multimodal, Vision, Language, Notes]
category: multimodal
draft: false
---

- 论文标题：《通过冻结图像编码器和大语言模型进行语言-图像预训练的自举方法》
- 动机：
	- 利用预训练的视觉模型和大语言模型
	- 在做生成任务前要先对齐和融合
- 方法：
	- 通过一个桥接模型连接冻结的视觉模型和大语言模型
	- 两阶段训练
		- 视觉-语言表示学习（文本和视觉特征的对齐和融合）
		- 基于图像的语言生成学习（训练模型的多模态生成能力）
- 模型架构
	- 第一阶段：视觉-语言表示学习
		- ITC任务![](/notes-images/2025-09-29_224808.png)
			- image encoder：32个维度为768的token
		- ITM任务![](/notes-images/2025-09-29_224748.png)
		- ITG任务（基于图像的文本生成）![](/notes-images/2025-09-29_224836.png)
		- 模型总览![](/notes-images/2025-09-29_224921.png)
			- 这里的self-attention是参数共享的，注意不同任务self-attention的mask是不同的
	- 第二阶段![](/notes-images/2025-09-29_225054.png)
- 训练细节![](/notes-images/2025-09-29_225218.png)