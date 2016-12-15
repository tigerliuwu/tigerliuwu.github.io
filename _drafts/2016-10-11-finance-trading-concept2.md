---
layout: post
title:  "Computational Investing"
categories: [Finance,Trading]
tags:  Finance
author: Wu Liu
---

* content
{:toc}
**一些关于金融投资的相关概念**



# Prerequisite
## Market mechanics

## What is a company worth
- **intrinsic value(固有值)**

根据每年派发的股息计算所得到的值。

例如：招商银行每年派发1元每股，其Discount rate（贴现率）为5%，那么该股的固有值为：\\(\\frac{1}{0.05}=20 RMB\\)。

- **book value(账面价值)**

总价值减去无形资产，再减去负债，即为账面价值。

例如：航空公司有10架飞机，每架飞机值100元；机场价值500元；商标价值100元；负债800元。
其中商标价值为无形资产。因此账面价值为：\\(10 * 100 + 500 - 800 = 700\\)

- **market capitalization(市值)**

上市公司在股票市场的总市值，即股票当前市价 * 股票发行数

# CAPM(The Capital Assets Pricing Model)

## introduction
- **Portfolio return**

\\(r_p(t) =\\sum_{i} w_i * r_i(t) \\)

其中，\\(w_i\\)为权重，即某一股票在整个资产组合(portfolio)中所占的百分比。
\\(\\sum_{i} abs(w_i)=1.0\\)

- **Market portfolio**

 - **Cap weighted**

  \\(w_i = \\frac{mktcap}{\\sum_{j}{mktcap}}\\)

- **CAPM**

$$
r_i(t) = \\beta_i r_m(t) + \\alpha_i(t)
$$

其中：\\(beta\\)即为跟SP500比较之后的slope。当SP500上升时，slope越大，股价上升的越快；
当SP500下降时，slope越小，损失越小。

![](/images/ML/stock/capm_figure_sample.jpg)

$$
r_p(t) = \\sum_i(beta_i r_m(t) + \\alpha_i(t))
       = \\beta_p r_m + \\alpha_p
$$


## how hedge funds use the CAPM


# build a model
## Technial Analysis
**侧重于使用股价和成交量来预测股价的走势**。

- **characteristics**
 - historical **price** and **volume** only
 - compute statistics called **indicators**
 - indicators are heuristic(启发式)

- **When is Technical analysis effective**
 - Individual indicators weak
 - combinations stronger
 - look for contrasts(stock vs market)
 - shorter time periods

- **when does technical analysis have value**


- **a few indicator**
 - **momentum**

$$
momentum(t) = \\frac{price(t)}{price(t-n)} - 1
$$

即：当天的动能由当天（t)的股价除以往前（t-n）天的股价。值越大越好。

其取值范围集中在：**[-0.5,0.5]**

 - **simple moving average**

例如：5日均线，10日均线，20日均线，30日均线

$$
SMA[t] = \frac{price[t]}{price[t-n:t].mean()} - 1
$$

其取值主要集中在**[-0.5,0.5]**

 - **Bollinger Bands(布林）**

$$
BB[t] = \frac{price[t] - SMA[t]}{2 * std[t]}
$$

取值范围集中在**[-1,1]**

![](/images/ML/stock/stock_BOLL_formula.jpg)


## Dealing with data

## Efficient Market Hypothesis(EMH)
**EMH thinks that both fundational&technical analysis are wrong.**

### EMH assumption
- large number of investors
- new information arrives randomly
- prices adjust quickly
- prices reflect all the available information

### origin of information
- price/volume
- fundamental
- exogenous
- company insider

### 3 forms of the EMH
- easy
- semi-hard
- hard

## The Fundamental Law of active portfolio management

### Grinold's Fundamental Law
- performance
- skill
- breath

$$
performance = skill * \sqrt{breath}
$$

eg. breath mean how many stocks you invest in and
skill means how you choose the stocks

## Portfolio optimization and efficient frontier










