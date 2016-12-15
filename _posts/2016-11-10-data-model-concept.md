---
layout: post
title:  "数据建模学习（三）-概念模型"
categories: LDM
tags: 逻辑数据模型
author: Wu Liu
---

* content
{:toc}

**overview**<br/>
概念模型比主题模型要更进一步，它的目的在于描述主要实体和这些实体之间联系。
概念模型反映了企业在运营过程中必须处理的**事情**。业务的事（business things）或事件（event）存在于办公地，非办公地以及公司外。
其中**事**是业务的核心，**事件**促使业务运作。这些业务的**事(things)**和**事件(events)**在ER图(entity-relationship diagram)中以实体出现。




# Things（事）
**Thing 从Party开始**
## Party

## Products

## Agreement

## Channels

## Location

## Campaign



# Events（事件）
**Events 在前面描述的 Things 之间发生，下面讲述的是Events发生的原因：**<br/>
**Note:**所有的事件都是围绕Agreements发生的事件。

## Usage
Party使用Agreement进行交易活动(transactions)。Agreement的用处在这里为附有跟该交易有关的金融机构利益，而且它提供了用户行为和渠道利用分析的基本信息。<br/>
Usage通常可被称为交易（transactions）或者事件（Event）。例如：取款，存款，支付。在保险行业里，保单的用处(usage)常为理赔(a claim).

## Inquiry（询问）
顾客做出询问(inquiry)，金融机构职员做出期望回覆。这种询问（inquiry）也可以理解为一个事件（Event）。询问可能涉及到协议（agreement）或者产品（product）。询问也可能涉及到卡的处理（如：结余查询，投诉，账户问题，产品信息以及利率信息等）。<br/>
询问（inquiries）和usage（使用）如果涉及到顾客联系，则可以认为两者都为联系事件（Contact Events）。Events（事件）可分组为一个会话（sessions）。一个会话可包括一个或多个Events。

## Maintenance
金融机构负责Agreements的日常维护（例如：计息(interest accrual),费用评估(fee assessment)，票据交换（check clearing))。考虑到维护影响到
Agreements，维护也认为是一个事件(Event)。

# ER digram
![](/images/LDM/LDM-figure-3-1.jpg)

