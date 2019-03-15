---
date: 2018-12-28 20:02
tags: DeepLearning
status: public
title: 'Deep Learning 100 Questions'
---

## 1、为什么用Label Smoothing ？
在分类模型中，模型预测的概率分布为$P(y'|x)$,数据的真实概率分布为 $q(y|x)$. 在训练模型时，我们采用one-hot的方式对真实分布进行编码，即观测样本属于哪一个类别，则对应类别的 $P(y|x)$的值为1，否则为0.
但这样的编码方式存在两个明显的问题：
+ 可能导致过拟合。由于0或者1的标记方式导致模型概率估计值更倾向于接近1，这种现象与真实分布不完全符合，有过拟合的风险。
+ 模型预测值趋向于1模型变得过于confident。 同样与实际不符。
采用的措施：

    $q'(y|x) = (1-\varepsilon)q_{x,y} + \varepsilon u(y)$
    $u(y)=1/K$
    
## 2、为何采用Xavier Initialization ？
通过使用这种初始化方法，我们能够保证输入变量的变化尺度不变，从而避免变化尺度在最后一层网络中爆炸或者弥散。
为了使网络中信息更好的流动，每一层输出的方差应该尽量相等。
可做如下推到：
对于某一层卷积：$y=w_1x_1+...+w_{ni}x_{ni}+b$
对于方差公式：$Var(w_ix_i)=E[w_i]^2Var(x_i)+E[x_i]^2Var(w_i)+VAr(w_i)Var(x_i)$

$Var(w_ix_i)=Var(w_i)Var(x_i)$
于是$Var(y) = n_iVar(w_i)Var(x_i)$
为了保证方差一至：$Var(w_i)=\frac{1}{n_i}$

同样为了保证前向传播和后向传播每一层方差一致，应满足：
$n_iVar[W^i]=1$
$n_{i+1}Var[W^i]=1$

同时考察两者后
$Var[W^i]=\frac{2}{n_i+n_i+1}$
又均匀分布概率：
$Var=\frac{(b-a)^2}{12}$

于是Xavier
$W~U[-\frac{\sqrt 6}{\sqrt{n_j+n_{j+1}}},\frac{\sqrt 6}{\sqrt{n_j+n_{j+1}}}]$

### 3、在词向量训练中如何处理out-of-vocabulary（OOV）问题？
+ Drop OOV words!
+ One OOV vector(unk vector)
+ à la carte Embedding（http://aclweb.org/anthology/P18-1002）
注：We propose an alternative, novel solution via a la carte ` embedding, a method which bootstraps existing high-quality word vectors to learn a feature representation in the same semantic space via a linear transformation of the average word embeddings in the feature’s available contexts.
+ Mimicking Embedding （http://www.aclweb.org/anthology/D17-1010） 2017
注：We train a recurrent neural network (RNN) on the character level with the embedding as the target, and use it later to predict vectors for OOV words in any downstream task. We call this model the MIMICK-RNN, for its ability to read a word's spelling and mimick its distributional embedding.
+ FastText                 2016
 Our method is fast, allowing to train models on large corpora quickly and allows us to compute word representations for words that did not appear in the training data.

### 4、1*1的卷积有何作用
+ 通过控制卷积核的数量大小，来控制通道数的大小。
+ 1X1的卷积过程相当于全连接层的计算过程，并且还加入了非线性链接函数，从而增加了网络的非线性。使得网络可以表达更加复杂的特征。
+ 减少参数
通过1*1的卷积减少网络的参数量。
例：

![](./_image/2019-03-05-20-15-25.jpg)
注：Previous layer维度是（$28*28*192$）
卷积核类型（$1*1*64,3*3*128,5*5*32$）
a图参数量 : （$1*1*192*64）+(3*3*192*128)+(5*5*192*32$)
b图参数量 ：
($1*1*192*64)+(1*1*192*96)+(1*1*192*16)+(3*3*96*128)+(5*5*16*32)+(1*1*192*32)$)

