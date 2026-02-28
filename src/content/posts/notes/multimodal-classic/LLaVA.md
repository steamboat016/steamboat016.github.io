---
title: "LLaVA"
published: 2025-02-26
description: "Notes on LLaVA — large language and vision assistant."
tags: [图文模型]
category: multimodal
draft: false
---

- 全称：Large Language and Vision Assistant
- 数据生成
	- prompt engineering![](/notes-images/2025-09-29_230532.png)
	- 数据示例![](/notes-images/2025-09-29_230626.png)
- 模型架构![](/notes-images/2025-09-29_230744.png)
	- 非常简单，基本就是线性投射层+LLM
- 两阶段训练![](/notes-images/2025-09-29_231209.png)
- LLaVA-1.5
	- 模型架构![](/notes-images/2025-09-29_231410.png)
		- 从1层线性层变成2层
	- 更丰富的数据
	- 支持更高的分辨率（ViT原本最高支持336x336）![](/notes-images/2025-09-29_231513.png)
	- 作者的思考：
		- 多模态大模型的幻觉问题可以通过提高图像分辨率大大缓解
		- 多模态大模型具有组合分项的能力，不需要构建融合各种能力的数据
			- 原本语言模型：长文本回答，多模态大模型：短文本+图像输入，仍然是长文本回答
- LLaVA-NeXT
	- 更高分辨率支持
	- 高质量的用户指令数据
	- 在更大的LLM上训练