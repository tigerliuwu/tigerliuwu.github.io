---
layout: post
title:  "Coursera:Machine Learning by Andrew Ng(4) - Neural Network"
categories: [machine learning]
tags: [coursera,machine learning]
author: Wu Liu
---

* content
{:toc}
**This blog's aim is demo the neural network, including both the algorithm and implementation**

# visualise 100 samples

## import the necessary modules


    #show the figures buildin the notebook
    %matplotlib inline 
    import numpy as np
    import matplotlib.pyplot as plt
    import scipy.io #Used to load the OCTAVE *.mat files
    import scipy.misc #Used to show matrix as an image
    from scipy import optimize
    import matplotlib.cm as cm #Used to display images in a specific colormap
    import random

## load the data from source file

```
    fileName = 'ex4/data/ex4data1.mat'
    mat = scipy.io.loadmat(fileName)
    
    X = mat['X']
    Y = mat['y']
    X = np.insert(X,0,1,axis=1)
    Y = Y - 1#Y[Y==10]=0   # convert 10 to 0
    print "'X' shape:%s.X[0] shape:%s" %(X.shape,X[0].shape)
    print "'y' shape:%s. Unique elements in y:%s" %(Y.shape,np.unique(Y))
```

**The output is:**

    'X' shape:(5000, 401).X[0] shape:(401,)
    'y' shape:(5000, 1). Unique elements in y:[0 1 2 3 4 5 6 7 8 9]


## visualization

### 3 steps to get done
1. reshape a sample to a 20*20 array
2. fill the returned array to a big array
3. visulize the big array

### problems
- from the result image, the vetical axis is not correct

```
    def getDatumImg(row):
        """
        function that is handed a single np array with shape 1*400,
        create an image from object from it, and return.
        """
        width, height = (20,20)
        return row[1:].reshape(width, height)
    
    def displayData(arr=None):
        """
        function that picks 100 samples from X to display as images.
        """
        width, height = (20,20)
        nrows, ncols = (10,10)
        if not arr:
            arr = random.sample(xrange(X.shape[0]),nrows*ncols)
        
        big_picture = np.zeros((height * nrows, width * ncols))
        
        irow, icols = (0,0)
        for idx in arr:
            if icols==ncols:
                irow = irow+1
                icols = 0
            sample = getDatumImg(X[idx])
            big_picture[irow * height:irow*height + sample.shape[0],icols*width:icols*width + sample.shape[1]] = sample.T
            icols = icols + 1
        fig = plt.figure(figsize=(8,8))
        img = scipy.misc.toimage(big_picture)
        plt.imshow(img,cmap=cm.Greys_r)
        plt.show()
        return
    
    displayData()
    print 'visualise the digit has done'
```
        


![png](/images/ML/NeuralNetwork_5_0.png)


    visualise the digit has done


# loss function

1. implement a loss function without penalty
2. implement a loss function with penalty

```
    def sigmoid(arr, theta):
        """
        function that sigmoid both the input samples and the parameters
        """
        z = np.dot(arr, theta)
        return 1.0 / (1 + np.exp(-z))
    
    def randInitializeWeights(input_layer_size, hidden_layer_size):
        """
        episilon = np.sqrt(6)/np.sqrt(Lin + Lout)
        Lin = the number of input layer unit
        Lout = the number of the adjacent layer unit
        """
        episilon = 0.12
        return np.random.rand(input_layer_size,hidden_layer_size+1) * 2.0 * episilon - episilon
    
    
    def sigmoidGradient(arr, theta):
        sig = sigmoid(arr, theta)
        return sig * ( 1 - sig)
    
    def reshapeParams(nn_params, input_layer_size=400, hidden_layer_size=25, num_labels=10):
        """
        function is used to reshape the input parameter:theta with type:list as 2 arrays, return it
        """
        print "the type of nn_params in reshapeParams is:%s" % type(nn_params)
        theta1 = np.array(nn_params[:(input_layer_size+1) * hidden_layer_size]).reshape((hidden_layer_size,input_layer_size + 1))
        theta2 = np.array(nn_params[-num_labels * (hidden_layer_size+1):]).reshape((num_labels, hidden_layer_size+1))
        return (theta1, theta2)
    
    def formatY(Y,num_labels=10):
        result = np.zeros((Y.shape[0],num_labels))
        for idx in xrange(Y.shape[0]):
            result[idx,Y[idx,0]] = 1
        return result
    
    def nnCostFunction(nn_params,  X, Y, lamda=0.0,input_layer_size=400, hidden_layer_size=25,
                       num_labels=10):
        """
        function to calculate the loss error of the samples
        """
        print "the type of nn_params in nnCostFunction is:%s" % type(nn_params)
        
        theta1, theta2 = reshapeParams(nn_params, input_layer_size, hidden_layer_size, num_labels)
        
        a1 = sigmoid(X, theta1.T) # m * hidden_layer_size
        a1 = np.insert(a1,0, 1, axis=1) # m * (hidden_layer_size + 1)
        #print a1[:10]
        #print "a1's shape:(%d,%d)" % a1.shape
        
        a2 = sigmoid(a1, theta2.T) # m * num_labels
        #print a2[:10]
        #print "a2's shape:(%d,%d)" % a2.shape
        
        # format Y from m * 1 to a m*num_labels array
        fY = formatY(Y,num_labels)
        #print "Y's shape:(%d,%d)" % fY.shape
        
        J = -(np.sum(np.log(a2[fY==1])) + np.sum(np.log(1.0 - a2[fY==0])))
        m = len(X)
        
        J = J/m + lamda * (np.sum(theta1**2) + np.sum(theta2**2)) /(2*m)
        print "cost value:%f" % J
        return J
    
    paramFile = 'ex4/data/ex4weights.mat'
    params = scipy.io.loadmat(paramFile)
    Theta1 = params['Theta1']
    Theta2 = params['Theta2']
    
    input_layer_size=400 # NO of features of samples
    hidden_layer_size=25 # NO of Hidden Units 
    num_labels = 10 # NO of Output Units
    
    #print "Theta1's shape:%s, Theta2's shape:%s" % (Theta1.shape, Theta2.shape)
    
    # flatten both Theta1 and Theta2 into one list
    theta = np.append(Theta1.flatten(),Theta2.flatten())
    #print type(theta)
    #print "theta size:%d" % theta.size
    
    #print X.dtype,Y.dtype,theta.dtype
    
    #theta1, theta2 = reshapeParams(theta)
    
    # if theta1=Theta1, theta2=Theta2, then the sum would be zero
    #print np.sum(theta1!=Theta1)
    #print np.sum(theta2!=Theta2)
    
    print nnCostFunction(theta,X,Y,0.0)
    
    print nnCostFunction(theta,X,Y,1.0)
```

**the output is:**
```
    the type of nn_params in nnCostFunction is:<type 'numpy.ndarray'>
    the type of nn_params in reshapeParams is:<type 'numpy.ndarray'>
    a1's shape:(5000,26)
    a2's shape:(5000,10)
    Y's shape:(5000,10)
    cost value:0.287629
    0.287629165161
    the type of nn_params in nnCostFunction is:<type 'numpy.ndarray'>
    the type of nn_params in reshapeParams is:<type 'numpy.ndarray'>
    a1's shape:(5000,26)
    a2's shape:(5000,10)
    Y's shape:(5000,10)
    cost value:0.384488
    0.384487796243
```

# back propagation

```
    def backpropagation(nn_params,  X, Y, lamda=0.0,input_layer_size=400, hidden_layer_size=25,
                       num_labels=10):
        theta1, theta2 = reshapeParams(nn_params, input_layer_size, hidden_layer_size, num_labels)
        a2 = sigmoid(X, theta1.T) # m * hidden_layer_size
        a2 = np.insert(a2,0, 1, axis=1) # m * (hidden_layer_size + 1)
        a3 = sigmoid(a2, theta2.T) # m * num_labels
        
        # format Y from m * 1 to a m*num_labels array
        fY = formatY(Y,num_labels)
        
        delta3 = a3 - fY   # m * num_labels
        delta2 = np.dot(delta3, theta2[:,1:]) * sigmoidGradient(X, theta1.T)   # m * (hidden_layer_size)
        
        grad2 = np.dot(delta3.T, a2) / X.shape[0] # num_labels * (hidden_layer_size+1)
        grad2[:,1:] = grad2[:,1:] + (lamda * theta2[:,1:]/X.shape[0]) 
        grad1 = np.dot(delta2.T, X) / X.shape[0] # (hidden_layer_size) * (input_layer_size+1)
        grad1[:,1:] = grad1[:,1:] + (lamda * theta1[:,1:]/X.shape[0])
        
        return np.append(grad1.flatten(),grad2.flatten())
    
    def computeNumericalGradient(mytheta, X, Y, mylambda=0.0,input_layer_size=400, hidden_layer_size=25,
                       num_labels=10):
        """
        mytheta is a flatten array
        """
        print input_layer_size,hidden_layer_size,num_labels
        print mytheta.shape
        ngrad = np.zeros((len(mytheta),1))
        episode = 0.0001
        for i in xrange(len(mytheta)):
            theta_plus = mytheta.copy()
            theta_plus[i]=theta_plus[i] + episode
            theta_minus = mytheta.copy()
            theta_minus[i] = theta_minus[i] - episode
            ngrad[i]=(nnCostFunction(theta_plus,  X, Y,mylambda,input_layer_size,hidden_layer_size,num_labels) - nnCostFunction(theta_minus, X, Y,mylambda,input_layer_size,hidden_layer_size,num_labels))/ (2 * episode)
        
        return ngrad
    
    def checkNNGradient(mylambda=0.0):
        input_layer_size = 3;
        hidden_layer_size = 5;
        num_labels = 3;
        m = 5;
        theta1 = randInitializeWeights(hidden_layer_size,input_layer_size);
        theta2 = randInitializeWeights(num_labels,hidden_layer_size);
        X = randInitializeWeights(m, input_layer_size - 1)
        X = np.insert(X,0,1,axis=1)
        Y = (np.arange(m) % 3).reshape(m,1)
        
        ngrad = computeNumericalGradient(np.append(theta1.flatten(),theta2.flatten()),X,Y,mylambda,input_layer_size,hidden_layer_size,num_labels)
        
        print ngrad.shape
        
        grad = backpropagation(np.append(theta1.flatten(),theta2.flatten()),  X, Y, mylambda,input_layer_size, hidden_layer_size,num_labels)
        print grad.shape
        #print (ngrad.flatten(),grad.flatten())
        print "%.15f" % (norm(ngrad.flatten() - grad) / norm(ngrad.flatten() + grad))
    
    def norm(arr):
        return np.sqrt(np.dot(arr,arr.T))
    checkNNGradient()
```

**then, the output is:**
```
    3 5 3
    (38,)
    the type of nn_params in nnCostFunction is:<type 'numpy.ndarray'>
    the type of nn_params in reshapeParams is:<type 'numpy.ndarray'>
    a1's shape:(5,6)
    a2's shape:(5,3)
    Y's shape:(5,3)
    cost value:2.088275

    the type of nn_params in nnCostFunction is:<type 'numpy.ndarray'>
    the type of nn_params in reshapeParams is:<type 'numpy.ndarray'>
    a1's shape:(5,6)
    a2's shape:(5,3)
    Y's shape:(5,3)
    cost value:2.088260
    (38, 1)
    the type of nn_params in reshapeParams is:<type 'numpy.ndarray'>
    (38,)
    0.000000000040026
```

```
    checkNNGradient(3)
```

```
    3 5 3
    (38,)
    the type of nn_params in nnCostFunction is:<type 'numpy.ndarray'>
    the type of nn_params in reshapeParams is:<type 'numpy.ndarray'>
    a1's shape:(5,6)
    a2's shape:(5,3)
    Y's shape:(5,3)
    cost value:2.121677
    the type of nn_params in nnCostFunction is:<type 'numpy.ndarray'>
    the type of nn_params in reshapeParams is:<type 'numpy.ndarray'>
    a1's shape:(5,6)
    a2's shape:(5,3)
    Y's shape:(5,3)
    cost value:2.121665
    (38, 1)
    the type of nn_params in reshapeParams is:<type 'numpy.ndarray'>
    (38,)
    0.106736304870728
```

```
    nnCostFunction(theta,  X, Y, lamda=3.0,input_layer_size=400, hidden_layer_size=25,num_labels=10)
```

**then the output is:**
```
    the type of nn_params in nnCostFunction is:<type 'numpy.ndarray'>
    the type of nn_params in reshapeParams is:<type 'numpy.ndarray'>
    a1's shape:(5000,26)
    a2's shape:(5000,10)
    Y's shape:(5000,10)
    cost value:0.578205
    0.57820505840604419

```

# build the model

```
    init_theta1 = randInitializeWeights(25,400)
    init_theta2 = randInitializeWeights(10,25)
    init_theta = np.append(init_theta1.flatten(),init_theta2.flatten())
    print len(init_theta)==len(theta)
    res = optimize.minimize(nnCostFunction,init_theta,args=(X,Y,1.0,400,25,10),method='BFGS',
                            jac=backpropagation,options={'maxiter':50,'disp':True})
```
# Predict

```
    ret_theta = res.x
    ret_theta1 = np.array(ret_theta)[:25*401].reshape(25,401)
    ret_theta2 = np.array(ret_theta)[25*401:].reshape(10,26)
    
    def predict(X,Y,ret_theta1, ret_theta2):
        a2 = sigmoid(X, ret_theta1.T)
        a2 = np.insert(a2,0,1,axis=1)
        a3 = sigmoid(a2, ret_theta2.T)
        ret = np.argmax(a3,axis=1).reshape(-1,1)
        ret = ret
        
        return np.mean(Y==ret) * 100
    
    print "the precision is:%f" % predict(X,Y,ret_theta1, ret_theta2)
```        
