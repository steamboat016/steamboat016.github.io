---
title: "Flamingo"
published: 2025-02-26
description: "Notes on Flamingo — few-shot visual language model."
tags: [Multimodal, Vision, Language, Notes]
category: multimodal
draft: false
---

- 论文标题：《一个小样本学习的视觉-语言模型》
- 动机
	- 支持多模态的few-shot learning
	- 利用已经训练好的视觉模型和大语言模型
- 模型特性
	- 桥接强大的视觉模型和大语言模型
	- 可以处理任意图片、文本、视频混杂的数据
	- 无缝接收图片/视频
- 模型架构![](/notes-images/2025-09-29_220705.png)
	- 图像特征提取![](/notes-images/2025-09-29_220744.png)
		- 这里的cross attention中，query是64个可学习token，key和value来自resnet提取展平的图像embedding
	- 视频特征提取![](/notes-images/2025-09-29_220802.png)
		- 和图像特征提取类似，不过给每一帧要加上时间编码，最终也是变成64个可学习token
		- 注意这里还有残差连接的设置
	- Gated Xatten-dense![](/notes-images/2025-09-29_221355.png)
		- 通过tanh门控函数，起初使得文本输入在经过前面的cross attention和ffw层时基本不发生改变，并且后面的大语言模型中的self attention和ffw层的参数是冻结的，随着训练改变tanh门控函数的参数，逐渐打开前面的cross attention和ffw层，也就是逐渐引入多模态信息
		- 另外这里的cross attention是masked cross attention，文本token仅能看到它前面的图片，实际上是只能和它前面的这一个图片做cross attention，但是由于整个token本身是要做self attention的，也就是说后面文本其实不仅能看到它前一个图片，也算是能看到前前的图片![](/notes-images/2025-09-29_222127.png)
	- 总结![](/notes-images/2025-09-29_221339.png)
		- 注意这里原本输入的是图文混排的信息，这里将图片提取传入vision encoder和perceiver sampler之后得到64个token，然后文本输入是将原本图片的位置改写为`<image>`，传入Gated Xatten-dense层等
- 缺点：
	- 不是一个纯粹的桥接器，perceiver resampler是，但是此外还有嵌入到大语言模型内的Gated Xatten-dense层，不够优雅，且会导致训练参数量大
	- 在生成任务之前没有做对齐和融合训练