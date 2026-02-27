---
title: "LLaMA 3.2 Vision"
published: 2025-02-26
description: "Notes on LLaMA 3.2 Vision — image encoder and tile-based processing."
tags: [Multimodal, Vision, Language, Notes]
category: Notes
draft: false
---

- 一个tile(块)的大小为560x560
	- tile有8种组合方式，根据输入图片，选择最合适的tile组合方式![](/notes-images/2025-09-29_234429.png)
		- 不改变宽高比，缩放后完全放入其中的一种，缩放最小的就是最合适的
		- 剩余部分用0padding
- image encoder![](/notes-images/2025-09-29_234127.png)
	- 这里图中笔误，是tile不是tail
- 模型架构总览![](/notes-images/2025-09-29_234157.png)
	- 这里的text encoder直接用冻结的LLama3.1，后面的某些层的门控cross attention和ffw是可训练的