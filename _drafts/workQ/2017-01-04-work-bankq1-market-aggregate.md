---
layout: post
title:  "征信增值产品 - 商业银行信贷组合查询(1) - 市场集中度"
categories: [credit service]
tags:  [work, problem]
author: Wu Liu
---

* content
{:toc}
- 产品适用对象：21家全国性银行总行
- 观测群体：7家中资全国性大型银行、5家主要商业银行、4家国有商业银行、12家全国性股份制商业银行、2家政策性银行 => 共5类银行；如果某一金融机构可以分属不同的分组，但金融机构用户查询时必须且仅能选择一组分类作为观测群体；如果某一金融机构仅属于一个分组中，则界面查询时不用选择观测群体，仅提示其所属的观测群体。





# 市场集中度

## 行业贷款市场集中度
- 功能
该功能是采用劳伦茨曲线的形式分地区展示行业贷款集中度，分别反映余额、发放额集中度。可反映同一时点不同行业贷款集中度，也可反映同一行业不同时点（时序）贷款集中度。

- 处理对象
处理**信贷业务=贷款业务**发放额、余额 分地区、分行业的汇总数据。

  - 涉及的维度：
    信贷业务（此处为**贷款**）、机构、行政区划（指金融机构所在地区）、行业维（按登记注册类型）、行业维(按贷款实际投向)、借款人规模、贷款性质、贷款期限、币种
  - 涉及的指标：
    余额，发生额
  - <font color='yellow'>时序值:</font>
    作为维度还是一个普通字段？

- 处理流程
用户查询行业贷款集中度， 1）选择地区（必选且单选），如北京；2）选择关注的行业（必选、多选且可跨行业类别），如农林牧渔业、采矿业等；3）选择指标（必选且单选），如发放额，同时确定币种（人民币、外币、本外币合计）；4）选择观测群体（条件选项）；5）增加三个可选条件，且支持多选：a.借款人规模（大型、中型、小型、微型、其他），b.贷款性质（自营贷款、委托贷款、特定贷款），c.贷款期限（短期、中期、长期）；6）选择时点值或时序值（必选且单选），如选时点值；7）提交任务后，生成相应图形。

- 结果图形

![贷款发放额集中度曲线](/images/work/creditService/loan_aggregate_curve.jpg)

**注：**图中曲线图横坐标为按贷款发放额从小到大的金融机构的排名序号（此处需要将贷款发放额进行数学转化再比较），纵坐标为各机构相对应的发放额占比的累计百分比，当在某一时段、某地区某行业仅有一家机构有贷款发放时曲线为一条折线，基尼系数等于1；当各家机构贷款发放额完全平均，曲线为一条对角直线，基尼系数等于0。

<font color='red'>
**problem:**
  - 制造业**为什么**向下弯曲，是不是因为发放额是负数？
  - 农、林、牧、渔业的曲线不连续，同时x=2到x=3开始于0结束于0，x=4也是从0开始的
</font>

- 其它说明
  - 处理对象：贷款业务， 分 发放额数据和余额汇总数据
  - 地区可选择：全国、31个省及直辖市、地市，共三个层次，必选且单选
  - 行业有两种：贷款实际投向行业和借款人所属的行业
  - 绘图：时点值，即选择月份，展示所有选择行业的市场集中度；时序值，按照每个行业分别绘制图形，展示该行业每个月份的市场集中度。


