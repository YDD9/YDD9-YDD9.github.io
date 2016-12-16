---
layout: post
title:  "Python scikit-learn ML"
date:   2016-12-16 14:58:39 +0100
categories: Python
---




# scikit-learn  
* Simple and efficient tools for data mining and data analysis  
* Accessible to everybody, and reusable in various contexts  
* Built on NumPy, SciPy, and matplotlib  
* Open source, commercially usable - __BSD license__   

# table of contents
[Installing the latest release](#install)  
[Data preparation](#dataprepare)   


## Installing the latest release <a name=install></a>

Scikit-learn requires:  
Python (>= 2.6 or >= 3.3),  
NumPy (>= 1.6.1),  
SciPy (>= 0.9).  

To verify a certain package among your installed packages via Cmder on windows:  

```
conda list | grep scipy
>> scipy    0.17.1   np111py27_1
```  

If you already have a working installation of numpy and scipy, the easiest way to install scikit-learn is using pip

```
pip install -U scikit-learn
```  

or conda:  

```
conda install scikit-learn  
```  


## Data preparation <a name=dataprepare></a>  

#### 1. Loading ready to use data from sklearn

```
from sklearn import datasets

# eg. Boston house price
boston = datasets.load_boston()
y = boston.target
X = bosston.data
```  



#### 2. Splitting your data
Learning the parameters of a prediction function and testing it on the same data is a methodological mistake: a model that would just repeat the labels of the samples that it has just seen would have a perfect score but would fail to predict anything useful on yet-unseen data. This situation is called overfitting. To avoid it, it is common practice when performing a (supervised) machine learning experiment to hold out part of the available data as a test set X_test, y_test  

```
sklearn.model_selection.train_test_split(*arrays, **options)  
```  

Parameters:	  
*arrays : sequence of indexables with same length / shape[0]  
Allowed inputs are lists, numpy arrays, scipy-sparse matrices or pandas dataframes.  

test_size : float, int, or None (default is None)  
If float, should be between 0.0 and 1.0 and represent the proportion of the dataset to include in the test split. If int, represents the absolute number of test samples. If None, the value is automatically set to the complement of the train size. If train size is also None, test size is set to 0.25.  

train_size : float, int, or None (default is None)  
If float, should be between 0.0 and 1.0 and represent the proportion of the dataset to include in the train split. If int, represents the absolute number of train samples. If None, the value is automatically set to the complement of the test size.  

random_state : int or RandomState  
Pseudo-random number generator state used for random sampling.  

Examples  

```
>>> import numpy as np
>>> from sklearn.model_selection import train_test_split
>>> X, y = np.arange(10).reshape((5, 2)), range(5)
>>> X
array([[0, 1],
       [2, 3],
       [4, 5],
       [6, 7],
       [8, 9]])
>>> list(y)
[0, 1, 2, 3, 4]
>>>
>>> X_train, X_test, y_train, y_test = train_test_split(
...     X, y, test_size=0.33, random_state=42)
...
>>> X_train
array([[4, 5],
       [0, 1],
       [6, 7]])
>>> y_train
[2, 0, 3]
>>> X_test
array([[2, 3],
       [8, 9]])
>>> y_test
[1, 4]
```  

#### 3. features normalization or standardization  





