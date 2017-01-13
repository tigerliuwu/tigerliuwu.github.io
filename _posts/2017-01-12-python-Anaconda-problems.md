---
layout: post
title:  "Anaconda(3) - problems"
categories: [Anaconda,Jupyter]
tags: [python]
author: Wu Liu
---

* content
{:toc}




# Jupyter notebook
1. **the modules in the created enviroment cannot be found in jupyter**

    there are 2 solutions as the following:
   * register a new kernel based on the environment:
```
    $ source activate stock
    $ conda install ipykernel
    $ ipython kernel install --name stock
``` 
   * install the jupyter notebook into the environment
```
    $ source activate stock
    $ conda install notebook
    $ jupyter notebook
```

2. **how to get the source code of a python object**

    need to make use of the module:**inspect**

```
    $ import inspect
    $ inspect.getsourcelines(object)
```

    for all the other functions in module:**inspect**, please refer to [inspect](https://docs.python.org/3/library/inspect.html)

# reference
- problem1: [stackoverflow:modules from conda env cannot be found](http://stackoverflow.com/questions/36382508/packages-from-conda-env-not-found-in-jupyer-notebook)
- problem2: [stackoverflow:get the source code for a python object](http://stackoverflow.com/questions/427453/how-can-i-get-the-source-code-of-a-python-function/)
