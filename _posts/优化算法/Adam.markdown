---
date: 2018-12-03 18:35
tags: 优化算法
status: public
title: Adam
---

# Adam
1、简介
Adam 在 RMSProp 基础上对小批量随机梯度也做了指数加权移动平均 [1]。下面我们来介绍这个算法。
2、Adam算法
使用动量变量$v_t$和RMSProp中小批量随机梯度按元素平方的指数加权平均变量$s_t$,并在时间步0将他们中每个元素初始化成0。给定超参数$0\leq \beta 1 <1$(建议0.9),时间步t的动量变量$v_t$即小批量随机梯度$g_t$的指数加权移动平均：
$v_t \leftarrow \beta_1 v_{t-1} + (1-\beta_1) g_t$
给定超参数$0\leq \beta 2 <1$（建议0.999）,将小批量随机梯度按元素平方后的项$g_t⊙ g_t$做指数加权移动平均得到$s_t$:
$s_t \leftarrow \beta_2 s_{t-1}+(1-\beta_2)g_t ⊙ g_t$
需要注意的是，当t较小时，过去各时间步小批量随机梯度权值之和会较小。例如当  $\beta_1=0.9,\beta_1=0.9$  时， $v_1=0.1g_1$ 。为了消除这样的影响，对于任意时间t，我们可以将vt 再除以  $1−\beta^t_1$，从而使得过去各时间步小批量随机梯度权值之和为 1。这也叫做偏差修正。在 Adam 算法中，我们对变量v_t和s_t均作偏差修正：

$v'_t \leftarrow \frac{v_t}{1-\beta^t_1}$
$s'_t \leftarrow \frac{s_t}{1-\beta^t_2}$

再用调整后的$v'_t,s'_t$将模型参数中每个元素的学习率通过按元素运算重新调整：

$g'_t \leftarrow \frac{\eta v'_t}{\sqrt{s'_1+\varepsilon}}$

于是随后使用：

$x_t \leftarrow x_{t-1}-g'_t$ 个更新自变量

3、超参数
$0\leq \beta 1 <1$ 建议0.9
$0\leq \beta 2 <1$ 建议0.999