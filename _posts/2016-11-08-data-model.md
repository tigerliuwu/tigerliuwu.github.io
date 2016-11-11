---
layout: post
title:  "数据建模学习（一）-逻辑数据模型图"
categories: LDM
tags:  逻辑数据模型
---

* content
{:toc}

概要：本文主要是介绍逻辑数据模型ER图，包括实体，联系的不同种类以及符号标记。




# 实体约定
![entity notation](/images/LDM/data_model_entity_notation.jpg)

## 说明：
1. **独立实体** <br/>
    不需要依赖其它的实体来对自身进行唯一性识别。方框中横线的上方属性为主键属性，下方为非主键属性。
2. **依赖实体** <br/>
    跟独立实体相对应，依赖实体需要其它的实体来对它自身进行唯一性识别。
3. **实线** <br/>
    连接两个实体，表示实体之间固有的联系。
4. **可选连线** <br/>
    最左侧的圆圈表示该联系可有可无，表示该联系不是必需的。
5. **虚线** <br/>
    表示子实体不依赖于父实体进行唯一性标识。
6. **外键** <br/>
    属性在该实体内是一个非主键属性，但是在某一个实体中，该属性必须是主键。

# 基数约定
下图展示了基数规则，描述了两个实体之间的联系。联系是基于当一个实体出现一次的时候，
与该实体相关的实体所出现的次数。<br/>
![cardinality notation](/images/LDM/data_model_cardinality_notation.jpg)

## 说明
1. 一对零、一或者多个
2. 一对多个
3. 一对零、一个
4. 一对三个
5. 一或零个对零、一或者多个

# 例子

1. **父子关系图**<br/>
子实体的存在以及唯一性标识都取决于父实体，如下图所示：<br/>
![Identifying Parent-Child Relationship](/images/LDM/LDM-figure1-1.jpg)

2. **其它标识性关系**<br/>
下图展示了其它的标识性关系。子实体的存在和唯一性标识都取决于父实体。如下图所示：<br/>
注意：子实体中唯一标识父实体的属性既是外键，又是主键。<br/>
![Other Identifying Parent-Child Relationship](/images/LDM/LDM-figure1-2.jpg)

3. **非标识性关系**<br/>
子实体的唯一性标识并不取决于父实体。此时，子实体中用于标识父实体的属性只是子实体的外键。
当父实体不存在的时候，该属性的值为null。如下图所示：<br/>
![Identifying Parent-Child Relationship](/images/LDM/LDM-figure1-3.jpg)

4. **互斥型关系**<br/>
父类型Account中所有的属性子类型Account Checking和Account Saving中都有；而子类型中的属性为各个子类型所特有的。
其中Account type表示该Account是Account checking还是Account Saving。针对某一个Account实例，
要么是一个Saving Account实例，或者Checking Account实例，要么根本不属于这两种子类型实例。<br/>
![Identifying Parent-Child Relationship](/images/LDM/LDM-figure1-4.jpg)

5. **内含型关系**<br/>
Event 有三种子类型：Account Event, Financial Event以及Contact Event。父类型Event中所有的属性都是子类型中共同的属性，
而子类型中的属性是各个子类型所特有的。下图表示一个Event实例可以都是它的三个子类型。<br/>
![Identifying Parent-Child Relationship](/images/LDM/LDM-figure1-5.jpg)






