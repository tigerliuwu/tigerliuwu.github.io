---
layout: post
title:  "Machine Learning(3) - feature normalization(feature scaling)"
categories: [machine learning]
tags: 机器学习系列
author: Wu Liu
---

* content
{:toc}

**Overview** <br/>
feature normalization:特征规范化，是用来对数据的独立特征进行范围标准化的一种方法，一般用于数据处理的前期工作中。



# 动机(motivation)
如果原始数据的各个特征之间取值范围相差特别大而且不经过规范化，在一些机器学习的算法中，目标函数(objective functions)就不能很好地工作。
例如：大部分的分类算法(classifier)需要计算两个点之间的Euclidean距离。如果其中的一个特征值特别大，而其它的特征值很小，那么这个距离就由该特征值来决定，其它的特征值基本被忽略了。**因此所有的特征值的取值范围都应该进行规范化(normalization)，从而使得每个特征值对最终的目标函数的影响都差不多**。
其次，**应用了feature normalization之后，优化算法：gradient descent就能更快地进行收敛(converge)**。

# 方法(methods)

## 尺度调整(Rescaling)
最简单的办法就是对所有的特征值进行范围调整，将他们的取值范围调整到［0，1］或者［-1，1］。将特征值具体调整到哪个目标范围取决于样本数据的特性。下面是一个通用的尺度调整方法：<br/>
$$
{x}' = \frac{x - min(x)}{max(x) - min(x)}
$$
<br/>
**说明：**
<br/>
其中，\\(x\\)是样本数据中的一个特征值，\\{x}'\\是该特征值规范化之后的结果。\\(min(x)\\),\\(max(x)\\)是该特征所有取值中的最小值和最大值。

## standardization(标准化)
经过feature standardization之后的样本，所有的特征的均值为零，该方法广泛用于音频和特想处理中。在机器学习算法(SVM,logistic regression, neural network)中都有广泛使用。具体个公式如下：<br/>
$$
{x}' = \frac{x - \bar{x}}{\delta}
$$
<br/>
**说明：**
其中\\(x\\)为样本的原始特征向量，\\(\bar{x}\\)为该原始特征向量的均值，而\\(\delta\\)为标准方差。
注：样本的原始特征向量为所有的样本中同一个特征所组成的向量。

## scaling to unit length（归一化到单位长度向量）
该方法是将样本的特征向量规范化后该特征向量的长度为1.方法如下：<br/>
$$
{x}' = \frac{x}{||x||}
$$
















