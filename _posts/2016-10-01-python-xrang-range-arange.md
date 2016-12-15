---
layout: post
title:  "Python tutorial - xrange,range,arange difference"
categories: [python]
tags: [python]
author: Wu Liu
---

* content
{:toc}
show the difference among xrange, range and numpy.arange




# xrange

format: \\(xrange(start,stop,step)\\)

xrange函数返回一个对象，如果只做循环使用，它比\\(range()\\)运行的时候速度更快，而且更节省空间。


    lp = xrange(1,10,2)
    for i in lp:
        print i

    1
    3
    5
    7
    9


# range

format：

> range(stop) --> integer of list (start = 0)
>
> range(start, stop, step) --> integer of list


    range(10)




    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]




    range(2,10)




    [2, 3, 4, 5, 6, 7, 8, 9]




    range(2,10,3)




    [2, 5, 8]



# numpy.arange

> numpy.arange(stop) --> integer of ndarray (start = 0)
>
> numpy.arange(start, stop, step, dtype=None) --> integer of ndarray


    import numpy as np


    np.arange(2,10,2)




    array([2, 4, 6, 8])


