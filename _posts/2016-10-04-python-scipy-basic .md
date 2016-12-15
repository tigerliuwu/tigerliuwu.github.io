---
layout: post
title:  "Python SciPy(1) - optimize"
categories: [python]
tags: [python, SciPy]
author: Wu Liu
---

* content
{:toc}
show the basic operation:optimize.minimize() of SciPy. It is great to have the build-in optimization function which I love much.

[optimize minimize](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.minimize.html#scipy.optimize.minimize)




## module:optimize

### function:minimize

**通过最小化成本函数，求取最优参数矩阵**

#### Definition
```
from scipy import optimize

optimize.minimize(fun, x0, args=(), method=None, jac=None, hess=None, hessp=None, bounds=None, constraints=(), tol=None, callback=None, options=None)
```

**说明：**
* **fun**:用于计算cost function的函数

* **x0**:原始参数矩阵\\(\Theta\\)

* **args**:除了原始参数矩阵\\(\Theta\\),其它必须传入到fun的参数需要罗列到此。

* **method**:采用何种优化方法，使得\\(\Theta\\)最优从而达到fun的返回值最小
  - Nelder-Mead
  - Powell
  - CG
  - BFGS
  - Newton-CG
  - L-BFGS-B
  - TNC
  - COBYLA
  - SLSQP
  - dogleg
  - trust-ncg
  - custom - a callable object(added in version 0.14.0)

* **jac**:成本函数的递减或者jacobian(Jacobian(gradient) of objective function)，只适用于CG, BFGS, Newton-CG, L-BFGS-B, TNC, SLSQP, dogleg, trust-ncg。<br/>
如果jac=True,则fun函数需要同时返回成本函数和其偏导数。如果jac=False,那么偏导数的判定通过数值运算来获得。<br/>
jac的值也可以是一个求成本函数的偏导数的函数，这时候，该函数必须和成本函数（fun）拥有相同的参数。

* **hess,hessp**:callable,optional
> Hessian (matrix of second-order derivatives) of objective function or Hessian of objective function times an arbitrary vector p.
> Only for Newton-CG, dogleg, trust-ncg. Only one of hessp or hess needs to be given. If hess is provided, then hessp will be 
> ignored. If neither hess nor hessp is provided, then the Hessian product will be approximated using finite differences on jac.
> hessp must compute the Hessian times an arbitrary vector.

* **bounds** : sequence, optional
> Bounds for variables (only for L-BFGS-B, TNC and SLSQP). (min, max) pairs for each element in x, defining the bounds on that
> parameter. Use None for one of min or max when there is no bound in that direction

* **constraints**:dict or sequence of dict, optional
> Constraints definition (only for COBYLA and SLSQP). Each constraint is defined in a dictionary with fields:
> **type : str** <br/>
> Constraint type: ‘eq’ for equality, ‘ineq’ for inequality.
> **fun : callable** <br/>
> The function defining the constraint.
> **jac : callable, optional** <br/>
> The Jacobian of fun (only for SLSQP).
> **args : sequence, optional** <br/>
> Extra arguments to be passed to the function and Jacobian.<br/>
> Equality constraint means that the constraint function result is to be zero whereas inequality means that it is to be non
> -negative. Note that COBYLA only supports inequality constraints.

* **tol**:float,optional <br/>
> Tolerance for termination. For detailed control, use solver-specific options

* **options** : dict, optional
> A dictionary of solver options. All methods accept the following generic options:
> **maxiter** : int
> Maximum number of iterations to perform.
> 
> **disp** : bool
> 
> Set to True to print convergence messages.

* **callback** :callable,optional
> Called after each iteration, as callback(xk), where xk is the current parameter vector.


**Returns**: **res**:OptimizeResult
> Important attributes are: x the solution array, success a Boolean flag indicating if the optimizer exited successfully and 
> message which describes the cause of the termination.

#### samples
```
def f(z):
    return 1.0/(1 + np.exp(-z))

def costFunction(theta, X, Y, mylambda=0.0):
    theta = theta.reshape(-1,1)
    Z = np.dot(X,theta)
    T = f(Z)
    result = np.sum(Y * np.log(T) + (1 - Y)*np.log(1-T)) * (-1) / len(X)
    if np.isnan(result):
        return np.inf
    return result
    
def gradient(theta, X, Y, mylambda=0.0):
    theta = theta.reshape(-1,1)
    #print theta, theta.shape
    Z = np.dot(X, theta)   # m*1
    T = f(Z)
    result = np.dot(X.T,T - Y) / len(X)  # n * 1
    return result.flatten()

result = optimize.minimize(costFunction,theta,args=(X,Y),method='BFGS',jac=gradient, options={'maxiter':400})

```

**得到的输出结果如下：**
```
status: 0
  success: True
     njev: 30
     nfev: 30
 hess_inv: array([[  3.07660923e+03,  -2.46603141e+01,  -2.46347979e+01],
       [ -2.46603141e+01,   2.11901191e-01,   1.84691126e-01],
       [ -2.46347979e+01,   1.84691126e-01,   2.12563326e-01]])
      fun: 0.20349770158966385
        x: array([-25.16136291,   0.20623191,   0.20147189])
  message: 'Optimization terminated successfully.'
      jac: array([  5.11757491e-08,   2.27330775e-06,   5.15598733e-06])
```

**说明：**

* success:True -表示该成本函数已经成功收敛(convergence).
* fun:0203.. -表示该成本函数在最优\\(\Theta=result.x\\)的最小值
* x:array.. - 最优参数\\(\Theta\\)
* jac:对成本函数最后一次求导所获得的偏导数

**注意**
* 参数矩阵参数:theta 传递到目标函数和求导函数之后，theta已经被flatten为一个一维Array，
因此需要将theta reshape为我们期望的矩阵，再进行后续的计算。
* 求偏导数的函数返回值，即为偏导数矩阵，也需要flatten为一维的Array。
* 目标函数和求偏导数的函数的参数必须一样。
