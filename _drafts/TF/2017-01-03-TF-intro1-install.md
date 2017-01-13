---
layout: post
title:  "tensorflow introduction(1) - install"
categories: [tensorflow]
tags: [tensorflow]
author: Wu Liu
---

* content
{:toc}
使用[virtualenv](http://docs.python-guide.org/en/latest/dev/virtualenvs/)来安装tensorflow



# current env
- os:ubuntu 14.04LTS
- python:2.7
- processor:Intel® Core™ i7-4710MQ CPU @ 2.50GHz × 8 
- graphics:Intel® Haswell Mobile 
- OS type:64-bit

# prepare the pip and virtualenv
## install pip and virtualenv
```
$ sudo apt-get install python-pip python-dev python-virtualenv
```

## create a virtualenv environment in the directory
```
$ virtualenv --system-site-packages ~/data_mining/tensorflow
```

## activate the environment
```
$ cd ~/data_mining/tensorflow/bin
$ source activate
```

## deactivate the environment
```
(tensorflow)$ deactivate
```

# install Tensorflow
```CPU
(tensorflow)$ pip install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.12.1-cp27-none-linux_x86_64.whl
```

```GPU
(tensorflow)$ pip install --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-0.12.1-cp27-none-linux_x86_64.whl
```

# startup with IPython
**make sure you've activate the tensorflow environment**

```
(tensorflow)$ ipython notebook
```

**note:**
Only startup ipython in the tensorflow environment with virtualenv, we can use tensorflow in the ipython.

# reference
- [tensorflow os_setup](https://www.tensorflow.org/get_started/os_setup#virtualenv_installation)
- [github tensorflow os_setup](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md)

