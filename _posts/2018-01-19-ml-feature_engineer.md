
---
layout: post
title:  "feature engineering(一) - basic"
categories: [machine learning]
tags: [machine learning]
author: Wu Liu
---

* content
{:toc}
对特征工程的概念说明以及针对对不同类型的特征进行加工的方法





# 1.feature(特征)
## 1.1定义
 - 特征是从原始数据提取出来的属性
 - 每个特征都是基于原始数据的一个特定的表示
 - 特征是一个独立可测量的属性，可以通过一个二维数据(m*1)来描述
 - 每一个观测都是一行记录，而特征则是这个观测一个列特定的值
 
## 1.2意义
 - 特征代表了用于训练模型所需要的信息
 - 特征的质量和数量对模型的效率有重要影响

## 1.3两种方式
 - 一种是固有的(inherent)特征，不需要对原始数据进行操作即可直接获取。例如根据原始数据：出生日期得到特征：年龄。
 - 另外一种是我们讨论的重点：基于多种数据类型，经过处理、抽取以及特征工程手段加工得到。

## 1.4步骤
 - 特征抽取和特征工程(feature extraction and engineering)
 - 特征缩放(feature scaling)
 - 特征选择(feature selection)
![images/MLpraticalML/feature_engineering.png]

**小结：**
 **特征工程就是利用经验、专业领域知识、直觉、数学转换等方法从原始数据中提取特征，从而更好地改进模型的执行效率，即：**
 - 更好地表示数据
 - 更好地改进模型效率
 - 对模型构建和评估意义非凡
 - 针对各种数据类型更加灵活
 - 强调了问题所在的业务和领域

# 2.feature engineering(特征工程)

## 2.1 feature engineering on numeric data



## 2.2 feature engineering on categorical data


## 2.3 feature engineering on text data


## 2.4 feature engineering on temporal data


## 2.5 feature engineering on image data


## 2.6 feature scaling


## 2.7 feature selection

## 2.8 dimensionality reduction