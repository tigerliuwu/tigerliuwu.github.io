---
layout: post
title:  "Anaconda(1) - introduction"
categories: [Anaconda]
tags: [python]
author: Wu Liu
---

* content
{:toc}
Anaconda为每个项目创建一个新的环境，将项目所需要的modules和其它项目所需要的modules隔离开来，从而避免因为各个项目因为不同的modules版本不同而产生冲突。

> Anaconda is an easy-to-install, free package manager, environment manager, Python distribution, and collection of over 150 open source packages with free community support.





# current env
- os:ubuntu 14.04LTS
- python:2.7
- processor:Intel® Core™ i7-4710MQ CPU @ 2.50GHz × 8 
- graphics:Intel® Haswell Mobile 
- OS type:64-bit

# installation
## download & install
Go to the Anaconda [download](https://www.continuum.io/downloads) to download it.

Python 3.5 version

```
bash Anaconda3-4.2.0-Linux-x86_64.sh 
```

Python 2.7 version
```
bash Anaconda2-4.2.0-Linux-x86_64.sh 
```

Note:during the installation, you can choose the path where you want to install it. I choose to install it in /opt/python.

## create a conda environment
This new created environment is used to store all the python libraries for your project.

```
$ conda create -n [project_name] python=[python_version]
```

- **project_name** is the project name(environment name),here we use **stock**.
- **python_version** : choose which python version for your project, like 2.7,3.5 and so on, here we choose 3.5.


**note:**

the new created env is located at:
```
~/.conda/env/
```

all the modules installed for the env is located in
```
~/.conda/envs/[env_name]/lib/python3.5/site-packages/
```
## activate and deactivate the environment
activate:

```
$ source activate stock
```

deactivate:
```
(stock)$ source deactivate
```

# install packages for env

**First**, we need to activate the environment(that's stock here)

**Then**, use pip to install the packages(or sometimes can use conda to install)

## use pip
- install
```
$ pip install [package_name]
```

- uninstall
```
$ pip uninstall [package_name]
```

note:
sometimes, pip may not work, there is still another way to install the moduel
 from the source code as the following:
```
$ python setup.py install
```
# startup with IPython
**make sure you've activate the tensorflow environment**

```
(stock)$ ipython notebook
```

**note:**
if this doesn't work, use the following command to install ipython:
```
(stock)$ conda install ipython
```

**or**
```
(stock)$ pip install ipython
```

# reference
- [tensorflow os_setup](https://www.tensorflow.org/get_started/os_setup#virtualenv_installation)
- [github tensorflow os_setup](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md)
- [python install modules](https://docs.python.org/2/install/)

