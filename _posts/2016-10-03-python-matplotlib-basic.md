---
layout: post
title:  "Python matplotlib - pyplot"
categories: [python]
tags: [python, pyplot]
author: Wu Liu
---

* content
{:toc}
show the basic operation of matplotlib.pyplot.

[pyplot document](http://matplotlib.org/api/pyplot_summary.html)




# basic operation

## plt.figure()
创建一个figure

## plt.subplot(nrow, ncols, npos)
将一个figure分成nrow*ncols，然后在第npos的位置制图.

eg. plt.subplot(2,2,1) or plt.subplot(221)：将当前的figure分为2行2列，并在第1个位置绘制图形。

## plt.plot(X,Y,*args,label='')
以数组X作为x轴，Y作为y轴，绘制图形。

eg. plt.plot(X,Y,'r+',label='red +') <br/>
在X，Y的交界点绘制红色的<font color='red'>+</font>

## plt.legend()
将plt.plot()中参数：label的值显示在图形的右上角。

## plt.show()
显示该figure

## plt.xlabel(str),plt.ylabel(str)
* plt.xlabel(str):x轴的标签
* plt.ylabel(str):y轴的标签

## plt.title(str)
figure的标题

## plt.contour(X,Y,Z,[values])
用于绘制等高线，其中values为需要显示的等高线的值

eg.
```
    mycontour = plt.contour(xvals, yvals, zvals,[0])
    myfmt = {0:'lambda = %d' % mylambda}
    plt.clabel(mycontour,inline=1,fontsize=15,fmt=myfmt)
```

只绘制zvals=0的等高线，该等高线设置的label为：**lambda=10**


**emphasis**
