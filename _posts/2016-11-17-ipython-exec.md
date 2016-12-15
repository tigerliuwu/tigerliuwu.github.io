---
layout: post
title:  "ipython notebook(2) - Operation"
categories: [python]
tags: [ipython notebook]
author: Wu Liu
---

* content
{:toc}
Introduce the operation when using **ipython notebook**, besides, record the possible problems I meet and solutions.




# startup
```
ipython notebook
```

# convert ipynb to other format
```
ipython nbconvert --to FORMAT filename.ipynb
```

convert the specific file:filename.ipynb to another Format(eg. html(default), pdf, **python, markdown**),
if you succeed, then you can find a same name file with different suffix in the same directory.

Here is an official document:[converting to other formats](https://ipython.org/ipython-doc/3/notebook/nbconvert.html)

## problems:

### missing module

> ImportError: No module named pygments

Solution:<br/>
```
sudo pip install pygments
```

if other modules are missing, the above solution can help solve it too.

### convert to python

* **convert to python3**

using 
```
ipython nbconvert --to python filename.ipynb
```


because I have %matplotlib inline in the ipynb file, then I get a python file with the first line:
> get_ipython().magic(u'matplotlib inline')

This is only supported in python3.*, but mine is python2.7.

<font color='red'>Solution:</font>


* **encoding**

When the *.py file include Chinese, poorly, when execute the python file, it shows up a encoding error message.

<font color='red'>Solution:</font>


# import python file into ipython notebook




