---
layout: post
title:  "tensorflow introduction(2) - first demo"
categories: [tensorflow]
tags: [tensorflow]
author: Wu Liu
---

* content
{:toc}




# first Demo Code

```
    # encoding: utf-8
    import tensorflow as tf


    tf.InteractiveSession()
```

**the output is:**

> <tensorflow.python.client.session.InteractiveSession at 0x7f031ebc1dd0>

```
    a = tf.zeros((3,2))
    b = tf.ones((3,2))
    tf.reduce_sum(b,reduction_indices=1).eval()
```

**the output is:**

> array([ 2.,  2.,  2.], dtype=float32)

```
    a.get_shape()
```

**The output is:**

> TensorShape([Dimension(3), Dimension(2)])

```
    tf.reshape(a,(1,6)).eval()
```
> array([[ 0.,  0.,  0.,  0.,  0.,  0.]], dtype=float32)

```
    print type(a), type(b)
```
> <class 'tensorflow.python.framework.ops.Tensor'> <class 'tensorflow.python.framework.ops.Tensor'>


# Conclusion
- 需要在tensorflow的VirtualEnv中启动Ipython
- tf.InteractiveSession()
    启动一个交互式的tensorflow session
- 如果需要查看一个tensor对象的value，使用该对象的eval()方法
- encoding: utf-8 =>用来解决中文乱码的问题
