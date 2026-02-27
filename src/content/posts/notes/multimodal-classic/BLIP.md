---
title: "BLIP"
published: 2025-02-26
description: "Notes on BLIP — bootstrapping language-image pre-training."
tags: [Multimodal, Vision, Language, Notes]
category: multimodal
draft: false
---

- 论文标题：《通过自举方式预训练的语言-图像模型，统一视觉-文本的理解和生成》
- 动机
	- 训练一个模型既能做检索任务又能做生成任务
	- 解决网络收集的图文对的噪声问题
- 模型架构![](/notes-images/2025-09-29_212713.png)
	- 做ITC任务时与ALBEF类似，也用了动量蒸馏
	- 做ITM任务时与ALBEF类似，也选择的是难负例
	- 做LM任务（给出文本，预测下一个token），采用label smoothing
		- e.g.避免采用one-hot过于绝对，取$\alpha=0.1$![](/notes-images/2025-09-29_213106.png)
	- 训练时一起训练，架构里相同的模块（图中相同颜色）共享参数
	- 以上模型就解决了第一个问题：一个模型做多种任务（检索+生成）
- 为解决第二个问题：网络图文对的噪声![](/notes-images/2025-09-29_213617.png)
	- 数据集分为网络图文对和人工标注的高质量图文对（如COCO数据集），先都用上进行预训练，然后仅用高质量数据集在ITC/ITM任务上训练做微调，得到**Filter**来做分类，同时仅用高质量数据集在LM任务上训练做微调，得到**Captioner**来输出图片描述（注意filter和captioner是对抗关系，不能共享参数，否则filter总会认为captioner的输出更好）
	- 用Captioner给网络图片生成文本描述，用Filter判断是Captioner生成的好还是原本网络图文对的文本好，如果是Captioner生成的好，文本描述就改用Captioner的，否则不变
	- 由此得到最终改善过的数据集，用这个数据集重新训练BLIP，即为较好的BLIP