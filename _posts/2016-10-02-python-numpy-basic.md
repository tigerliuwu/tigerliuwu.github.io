---
layout: post
title:  "Python numpy - basic"
categories: [python]
tags: [python, numpy]
author: Wu Liu
---

* content
{:toc}
show the basic operation of numpy.

[numpy document](https://docs.scipy.org/doc/numpy/reference/)



# 基本操作
## np.dot(a,b,out=None)
**数组a,b的内积**
<br/>

**例子:**
<br/>
```
>>> np.dot(3, 4)
12

>>> a = [[1, 0], [0, 1]]
>>> b = [[4, 1], [2, 2]]
>>> np.dot(a, b)
array([[4, 1],
       [2, 2]])
```

## numpy.sum(a, axis=None, dtype=None, out=None, keepdims=False)
**对axis指定方向（行或列）进行求和，如果axis没有指定，那么求数组a的和**
<br/>
- **参数说明：**
<br/>
  - **a**:array-like
  - **axis**:
  - **dtype**:用于指定返回计算结果的类型
  - **out**:ndarry,optional<br/>
   用于指定返回的结果数组，其中里面的维度必须和返回结果一样，注意：out数组里面的值会被替换掉。
  - **keepdims**:bool,optional<br/>
   如果设置为True，那么结果数组将会和输入数组的维度是一样的。
- **Returns:sum_along_axis**:ndarray <br/>
  如果out指定，那么返回的值为out的引用。

- **examples** <br/>
```
>>> np.sum([])
0.0
>>> np.sum([0.5, 1.5])
2.0
>>> np.sum([0.5, 0.7, 0.2, 1.5], dtype=np.int32)
1
>>> np.sum([[0, 1], [0, 5]])
6
>>> np.sum([[0, 1], [0, 5]], axis=0)
array([0, 6])
>>> np.sum([[0, 1], [0, 5]], axis=1)
array([1, 5])
```

## np.invert(a[,out])
**对数组a取非,等同于np.bitwise_not**
<br/>
We’ve seen that 13 is represented by 00001101. The invert or bit-wise NOT of 13 is then:
```
>>> np.invert(np.array([13], dtype=uint8))
array([242], dtype=uint8)
>>> np.binary_repr(x, width=8)
'00001101'
>>> np.binary_repr(242, width=8)
'11110010'
```

结果依赖于数据拥有多少位（bit-width)
```
>>> np.invert(np.array([13], dtype=uint16))
array([65522], dtype=uint16)
>>> np.binary_repr(x, width=16)
'0000000000001101'
>>> np.binary_repr(65522, width=16)
'1111111111110010'
```

也可以对bool数组求非：
```
>>> np.invert(array([True, False]))
array([False,  True], dtype=bool)
```


## np.argmax(min)(a, axis=None,out=None)
**根据axis指定的方向寻找最大值（最小值），返回最大值所在的索引。如果指定了axis，那么返回一个darray数组，如果没有，则返回一个int

**参数说明**
> **a**:array_like
> **axis**:指定寻找方向。axis = 0，沿着行的方向找最大值；axis = 1,沿着列的方向寻找最大值。
> **out**:array。如果指定，则返回结果直接插入到该数组对象。

```
>>> a = np.arange(6).reshape(2,3)
>>> np.argmax(a,axis=1)
array([2,2])
>>> np.argmax(a,axis=0)
array([1,1,1])
>>> np.argmax(a) # first flatten the source array, then look for the max value
5
```

## np.insert(arr,obj,value,axis=None)
**在axis指定的方向插入value，如果axis没有指定，那么首先对arr进行拍平(flatten)**
<br/>
- **parameters:**
 - arr:array_like
 - obj:int, slice or sequence of ints <br/>
 - value:array_like
 - axis:int,optional
- **Returns: out**:ndarray

**例子：**

```
>>> a = np.array([[1, 1], [2, 2], [3, 3]])
>>> a
array([[1, 1],
       [2, 2],
       [3, 3]])
>>> np.insert(a, 1, 5)
array([1, 5, 1, 2, 2, 3, 3])
>>> np.insert(a, 1, 5, axis=1)
array([[1, 5, 1],
       [2, 5, 2],
       [3, 5, 3]])
>>> b = a.flatten()
>>> b
array([1, 1, 2, 2, 3, 3])
>>> np.insert(b, [2, 2], [5, 6])
array([1, 1, 5, 6, 2, 2, 3, 3])
```



# ndarray
**emphasis**
