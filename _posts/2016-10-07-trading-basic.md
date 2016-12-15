---
layout: post
title:  "Trading Basic (1)"
categories: [finance,trading]
tags: [finance]
author: Wu Liu
---

* content
{:toc}





# Trading rolling average

## 均线(mean price)
**N日均线:**

当前交易日以及前面（N-1)个交易日的股价总和除以N，即\\(\\frac{\\sum_{i=1}^{i=N}price}{N} \\)。

## 布灵线(BOOL)
英文全称：**Bollinger Bands**,根据标准方差计算得来。
绘图时，在平均线上[ \\(-2\\delta,+2\\delta\\)]，即可得到BOOL线。

## 夏普比率(Sharpe Ratio)
**核心思想：理性的投资者将选择并持有有效的投资组合，**
**即那些在给定的风险水平下使期望回报最大化的投资组合，**
**或那些在给定期望回报率的水平上使风险最小化的投资组合。**

换句话说，投资者建立有风险的投资组合时，至少应该要求回报达到无风险投资的回报，甚至更多。

**formula:\\(\\frac{mean(Rp - Rf)}{\\delta p}\\)**

其中
- Rp:是投资组合预期报酬率<br/>
- Rf:无风险利率
- \\(\\delta p\\):投资组合的标准差

举例而言，假如国债的回报是3%，而您的投资组合预期回报是15%，您的投资组合的标准偏差是6%，那么用15%－3%,可以得出12%（代表您超出无风险投资的回报），再用12%÷6%＝2，代表投资者风险每增长1%，换来的是2%的多余收益。

夏普理论告诉我们，投资时也要比较风险，尽可能用科学的方法以冒小风险来换大回报。所以说，投资者应该成熟起来，尽量避免一些不值得冒的风险。

refer to:<http://www.iwms.net/n1858c38p2.aspx>


