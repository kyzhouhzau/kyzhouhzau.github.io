---
date: 2018-12-03 16:56
tags: 优化算法
status: public
title: Adadelta
---

# Adadelta
1、解决的问题同RMSProp一样
2、Adadelta算法
Adadelta算法也像PMSProp一样，使用了小批量随机梯度$g_t$按元素平方的指数加权移动平均变量$s_t$。在时间步0，他的所有元素被初始化成0。给定超参数$0\leq \gamma <1$,在时间$t>0$,同RMSProp一样计算
$s_t \leftarrow \rho s_{t-1}+(1-\rho)g_t ⊙ g_t$
与RMSPorp不同的是，Adadelta还维护一个额外的状态变量$\Delta x_t$来计算自变量：
$g'_t \leftarrow \sqrt{\frac{\Delta x_t-1+\varepsilon}{s_t+\varepsilon}}⊙g_t$

于是自变量更新为：
$x_t \leftarrow x_{t-}-g'_t$

$\Delta x_t \leftarrow \rho x_{t-1}+(1-\rho)g'_t ⊙ g'_t$
可以看到，如不考虑ϵ的影响，Adadelta 跟 RMSProp 不同之处在于使用  $\sqrt{Δx_t−1}$  来替代超参数η。

3、超参数

$0\leq \rho <1$

$\varepsilon$   倾向选小值$1e^{-6}$