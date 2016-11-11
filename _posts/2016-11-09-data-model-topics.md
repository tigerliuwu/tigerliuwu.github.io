---
layout: post
title:  "数据建模学习（二）-主题模型"
categories: LDM
tags: 逻辑数据模型
author: Wu Liu
---

* content
{:toc}

**overview**<br/>
对主题模型中十大主题进行描述




# model components
* Foundation
* Banking
* Insurance
* Investments

# 主题域模型
![](/images/LDM/data_model_concept.jpg)
<br/>

## The ten major subject area:
* party
* party asset
* product
* agreement
* internal organization
* event
* location
* campaign
* channel
* finance

### Party
    Party可以是任何个人或者任何组织，也可以是任何可以跟金融机构有业务往来的合法团体。

### Asset
    Asset是资产，是Party所拥有的股票等有价值的东西。当资产被作为贷款抵押物的时候，投保部分依然可以作为Party的一部分财富。

### Product
    产品可以是任何可销售产品或者服务，这种产品必须由金融机构和保险公司出售给客户进行使用。
如果需要的话，也可以将其它的金融相关的产品包含进来，例如存单(certificate of deposit), 活期账户(checking accounts),
贷款(loans)，保单(insurance polices)，保险箱(safe deposit boxes)，汇票(money orders),公证(notary service),
stocks（股票），债券(bonds)。
    产品也可以是一系列产品构成的产品包。
    产品通过特征(features)来描述。特征包括：balance limits,fee,term,interest rates, insurance coverages,
coverage limits, co-pays, and deductibles(免赔额).

### Agreement
    协议是为了提供一种产品或者服务而在金融机构和客户之间达成的一种持续的契约或者交易。
例如：活期账户(checking account)、储蓄账户(saving account)、保单(insurance policy)、定期存款账户(time deposit account)，
按揭贷款(mortgage loan)、投资账户(investment account),也包括健康提供商协议(health care provider contract agreements)。

### Internal Organization
> An Internal Organization is an internal unit of a business that the financial
> institution is interested in tracking. Examples include branch, call center,
> sales district, region, group, store and function.
> Internal Organization is not a legal structure and cannot
enter into a contract on its own.
<br/>

### event

    事件是银行与客户或潜在客户之间的联系或交易活动，它记录了详细的行为和交易数据，包括存取款、收费、计息、咨询投诉、查询、市场调查、网上交易等。
可能需要也可能不需要银行与客户的直接接触；可能与帐户相关，也可能与帐户无关；可能与资金相关，也可能与资金无关。
通过事件可以帮助了解哪些客户使用哪些渠道做哪种交易事件，交易金额多少、什么时间、在什么地点、与金融机构的哪位员工或部门打交道。

### location
> A location is an address. The address may be a geographical area, a street
> address, parcel of land, a post office box, a telephone number or an electronic
> address such as an internet address. A geographical area is any bounded area
> on the earth such as city, state, province, census block, and postal area.<br/>

	LOCATION主题是指银行希望关注或考察的任何层次的地理区域和地址。如国家、省份、城市、县、乡村等。
LOCATION主题包含“具体地址”、“地区”、“地理位置”等不同层次的信息.
该主题和事件、产品、渠道、内部组织机构、营销活动等主题都有着密切的联系。

### campaign
    营销活动是为了获取、维护、增强银行与客户的关系而开展的一些促销的活动；
营销活动是一些有组织的活动，其目的可以是为了把某些产品推向市场，也有可能是为了树立银行在市场上的形象；
完整的营销活动应该包括营销策略、营销行为以及营销活动的反馈信息；
收集营销活动的信息可以帮助银行发现最有效的营销方式，了解不同类型客户对营销活动的反馈.

### channel
    用户通过渠道向金融机构获取关金融机构或金融机构产品信息以及使用金融产品。金融机构通过渠道向用户销售产品或提供服务。
渠道与当事人、产品、帐号等其他实体存在各种关系。
渠道分为若干渠道类型。


### finance

> Major Features
> Tracks General Ledger and Journal Voucher information
> GL Subtyping - Assets, Liabilities, Equity, Revenue and Expense
> GL Balance over time - for budget and actual
> Link to Transaction Event and customer Account/Policy
> To provide for drill down from GL to Agreement and to transaction Event
> Example - if customer made a premium payment (Event) then this would result in  debit and credit journal vouchers posted to the ledgers.
> Provides for flexibility in the GL numbering scheme.

## 各主题之间的业务规则

### Party-Product关系

    多对多的关系。
    Party为客户时，会购买或遗赠产品。<font color='red'>该实体</font>来源于可预测模型的输出。
Party为组织或者企业时，给客户提供产品。一些金融机构的分支点也可以提供所有的产品；而其他的分支点只可以提供主要产品。
一些汽车经销商可以替金融机构提供汽车贷款。一些保险经纪人可提供所有的保险产品，而有些保险经纪人只能提供汽车保险。
Party为雇员时，该雇员则有权协商费用来加大产品的消费量。

### Party-Agreement 关系
> (Agreement can be banking account, an investment agreement or an insurance
> agreement, including health care provider contracts)
> A Party can have many Agreements, and an Agreement can be related to one
> or more Parties as in the case of joint accounts.
> Parties can have different roles with different Agreements.
> A Party may be related to an Agreement as an account holder.
> A Party may be related to an Agreement not as an account holder or customer
> but in another role such as beneficiary, co-signer, or reference.
> A Party that is a Business may issue credit cards.
> A Party can receive funds on a regular basis (standing orders) from zero, one
> or many Agreements.
> An Agreement is related to a Party that is the financial institution’s Internal
> Organization for many reasons.
> • It is the organization that originated the account.
> • It is the organization that services the account.
> A Party that is an employee has a relationship with many agreements (e.g.
> primary service rep), and an agreement can have relationships with many
> employees for different reasons (e.g. for sales or for services).

### Party-Asset关系
    抵押物也是Asset，跟Agreement和Customer都有关联。
Parties可能对asset具有所有权，担保权、租赁权。一个asset可以为不同的parties所部分拥有或者全部拥有。
例如房子，车子属于party，同时又可以投保。

### Party-Internal Organization Relationships
A Party that is an Individual can also be an Employee of a Financial
Institution (Party Relationship).
A Party that is an Internal Organization may house a financial institution’s
Organizational Unit. The organization can be independent of the financial
institution, as in the case where an airport or supermarket houses a branch.
Over time, a Party that is a Customer can have many relationships with
Parties that are an Internal Organization.
A Party that is a Customer can have many relationships with Parties that are
Employees (such as primary service representative, primary sales
representative, and loan consultant.)

### Party-Event Relationships
A Party is involved in zero, one or many Events with the financial institution.
In the case of credit or debit card events, a Party may be the merchant for
zero, one or many financial account events with the financial institution.
A Party that is a Financial Institution is the other financial party involved in a
Check Financial Account Transaction.
An event that is an insurance claim involves zero, one or many Parties that are
subject of the claim, and one or many Parties that are submitter of the claim.
An event that involves a contact with a customer involves zero, one or many
Parties that are internal organizations of the financial institution.

### Party-Location Relationships
A physical address, over time, is occupied by one or more parties.
An electronic address can be used by one or more parties.

### Party-Campaign Relationships
A Party is assigned to zero, one or many market segments.
A Party is targeted by zero, one or many campaigns.
A Party has a response score for a promotion.
A campaign targets zero, one, or many Parties that are a financial institution
organization such as sales district, region or branch. This would be used, for
example, if a promotion were for signage at various branches.
A marketing segment associated with a campaign is created by one employee.
### Party-Channel Relationships
A Party may have many roles with many channels (houses, owns). The Party
need not be the financial institution; e.g. a supermarket that houses an ATM, a
POS or a kiosk.
A business manufactures zero, one or many ATM devices. An ATM device is
manufactured by one manufacturer.
A customer has zero, one or many sales channel types that are preferred for
solicitation purposes.
Channels may be related to many Parties for different roles. For example, an
ATM channel is housed by one financial institution organization and managed
by another.

### Product-Agreement Relationships
An agreement at any one time is related to only one Product. Over a period of
time, an Agreement may be related to more than one Product. An example of
an Agreement that over time can be related to many Products is when a
Certificate of Deposit is renewed under new terms and conditions with a
different product identifier but the same account number.
The Agreement is also related to the Package of which the Product is a part.
A Product for an Agreement does not have to be part of a Package.

### Product-Event Relationships
A contact Event with a customer involves zero, one or many products.
One script sequence can be used when selling one product.
### Product-Location Relationships
A Product, such as travel insurance, may be valid in one or many
geographical areas.
### Product-Campaign Relationships
A Product is marketed via zero, one or many campaigns. A Campaign
markets zero, one, or many Products.
A Promotion may target zero, one or many Cluster (Party) Groups.
A Feature may be a selling point in zero, one or many Promotions, and a
Promotion may have zero or many Features that are selling points. For
example, a feature that is a selling point may be the interest rate or the waiver
of an annual fee for the first year.

### Product-Channel Relationships
A Product may be sold or serviced via one or many channel types.

### Agreement-Event Relationships
An Event involves zero or one Agreements.
An Event may be a financial event that affects the balance of an Agreement
such as withdrawal, deposit, loan payment, premium payment, and claim
payout.
A Contact Event may involve a Card. Examples are credit card transaction or
ATM withdrawal.

###Agreement-Location Relationships
An Agreement may have its statements sent to multiple addresses.

### Agreement-Campaign Relationships
An Agreement was created as a result of zero or one campaign.

### Agreement- Channel Relationships
The costs of an Agreement are allocated to zero, one or many channels.
An Agreement is to be used at zero, one or many specific Channels.
An Agreement has its primary activity at zero or one specific Channel.



### Event-Campaign Relationships
> An event involving a contact with a customer is the result of zero or one campaign.

### Event-Channel Relationships
> A Contact Event involves one channel.
> Location-Campaign Relationships
> A Campaign targets zero, one, or many geographical areas.
### Location-Channel Relationships
> There are no Location-Channel relationships. The physical address of the
> channel can be derived from the business or internal organization that houses
> it (see Party-Channel).


### Campaign-Channel Relationships
> A Campaign at all levels may use many channel types. For example, a plan
> may use radio, television and mail.
> A Campaign at the lowest level of the Campaign plan may use one or more
> specific channels. For example, a birthday promotion may target a group of
> customers at specific ATMs for happy birthday messages.


### Finance-Agreement Relationships
> A general ledger account is assigned to zero, one or many AGREEMENTS.
> Over time, an Agreement may relate to more than one general ledger account.

### Finance-Event Relationships
> An EVENT may result in internal journal entries that get posted to the ledger.
> For example, a deposit to a bank account results in a debit and credit journal
> entry.


