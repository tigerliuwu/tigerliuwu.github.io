---
layout: post
title:  "Pandas basic - DataFrame(1)"
categories: [python,pandas]
tags:  python
author: Wu Liu
---

* content
{:toc}
**how to download stock data:**
- by url:<http://wenku.baidu.com/view/fd408c4ee518964bcf847ca3.html>
- by java:<https://my.oschina.net/875881559/blog/99729>

**import pandas as pd** to import pandas library.




# DataFrame
一个二维、大小可变的、扁平的表型数据结构；它也有轴线，即行和列，因此可以沿着行和列进行数据运算。
可以认为这是一个类似于字典的序列对象容器。是pandas最主要的数据结构。

**[DataFrame](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html)**

**see also**
- **[DataFrame.from_records](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.from_records.html#pandas.DataFrame.from_records)**
constructor from tuples, also record arrays
- **[DataFrame.from_dict](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html)**
from dicts of Series, arrays, or dicts
- **DataFrame.from_items(http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.from_items.html#pandas.DataFrame.from_items)**
from sequence of (key, value) pairs
- **pandas.read_csv, pandas.read_table, pandas.read_clipboard**


## how to create a DataFrame
### from a source file
- **pd.read_csv(filePath)**

如果文件以逗号：**,**进行字段分割，可以直接使用上述代码从源文件中抽取数据构成一个DataFrame对象。

**[read_csv](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html#pandas.read_csv)**

**pd.DataFrame()
- **Parameters:**

 - **data** : numpy ndarray (structured or homogeneous), dict, or DataFrame
Dict can contain Series, arrays, constants, or list-like objects
 - **index** : Index or array-like
Index to use for resulting frame. Will default to np.arange(n) if no indexing information part of input data and no index provided
 - **columns** : Index or array-like
Column labels to use for resulting frame. Will default to np.arange(n) if no column labels are provided
 - **dtype** : dtype, default None
Data type to force, otherwise infer
 - **copy** : boolean, default False
Copy data from inputs. Only affects DataFrame / 2d ndarray input

- **examples**
```
left = pd.DataFrame({'A': ['A0', 'A1', 'A2'],
                         'B': ['B0', 'B1', 'B2']},
                         index=['K0', 'K1', 'K2'])
```

||A|B|
|:---:|:---:|:---:|
|K0|A0|B0|
|K1|A1|B1|
|K2|A2|B2|


```
>>> other = pd.DataFrame({'key': ['K0', 'K1', 'K2'],
                       'B': ['B0', 'B1', 'B2']})
>>> other
    B key
0  B0  K0
1  B1  K1
2  B2  K2
```

## pd.fillna(method=None,inplace=True)
- **method='ffill'**

  向前填充空值

- **method='bfill'**

  向后填充空值

- **inplace=True**

  直接修改DataFrame里面的数据，从而影响跟该DataFrame相关的所有的视图等。


