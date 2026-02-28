---
title: "Optimizer"
published: 2025-02-26
description: "指数加权平均、动量、RMSProp、Adam 与权重衰减。"
tags: [优化器]
category: deeplearning
draft: false
---

### 引入：指数加权平均及其改进
- 思想：参照历史数据，并且越新的数据越有参考价值（权重越大）
- 利用指数加权平均，对历史数据加权平均，同时老数据的权重是指数衰减的
$V_0=0$
$V_t=\beta V_{t-1} +(1-\beta)\theta_t$
- 该过程中可以仅用一个变量$V$
- 问题：由于初始设置$V_0=0$值较小，得到的结果也会偏小，在很多次迭代后才会接近真实值
- 改进：除以$1-\beta^t$，将预测值变大，同时在很多次迭代后($t\rightarrow\infty$)，这样的修正基本不改变预测值，也就是仍然会接近真实值
$V_t^{correct}=\frac{V_t}{1-\beta^t}$
### 动量梯度下降
- Momentum Gradient Descent
- 缓解梯度正负震荡，利用惯性加速收敛
$g_w=\frac{\partial loss}{\partial w}$
$V_w=\beta V_w+(1-\beta)g_w$
$w=w-lr\cdot V_w$
### RMSProp优化器
- Root Mean Square Propagation optimizer
- 让每个参数有自适应的学习率
	- 让梯度值大的参数的学习率相对小一些
	- 让梯度值小的参数的学习率相对大一些
- 思路：学习率除以自身的历史梯度信息
$g_w=\frac{\partial loss}{\partial w}$
$S_w=\beta S_w+(1-\beta)g_w^2$
$w=w-\frac{lr}{\sqrt{S_w}+\epsilon}g_w$
### Adam优化器
- Adaptive Moment Estimation
- 结合以上二者，同时利用动量来给梯度更新增加惯性和震荡阻尼，也利用历史梯度的均方根来自适应调整学习率；同时还对动量梯度和参数梯度平方的指数加权平均值进行修正
$g_w=\frac{\partial loss}{\partial w}$
$V_w=\beta_1 V_w+(1-\beta_1)g_w$
$S_w=\beta_2 S_w+(1-\beta_2)g_w^2$
$V_w^{correct}=\frac{V_w}{1-\beta_1^t}$
$S_w^{correct}=\frac{S_w}{1-\beta_2^t}$
$w=w-lr \frac{V_w^{correct}}{\sqrt{S_w^{correct}}+\epsilon}$
### 权重衰减
$w=w-lrg_w-lr\lambda w$
- 简单推导易得：在标准梯度下降算法中，使用权重衰减和L2正则化是等价的