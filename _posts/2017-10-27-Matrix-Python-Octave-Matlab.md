http://mathesaurus.sourceforge.net/matlab-numpy.html   
One to One correspondance function between Matlab/Octave V.S. Numpy in Python
```
# A, B are Matrix

A\B   ---> linalg.solve(A,B) when A is square matrix; linalg.lstsq(A,y) A is not square matrix
# https://stackoverflow.com/questions/7160162/left-matrix-division-and-numpy-solve

A'    ---> transpose(A) or A.T
A./B  ---> A / B

diag(A)   ---> diagflat(A)
A .^2     ---> A **2
sqrt(A)   ---> sqrt(A)

```

https://www.ibm.com/developerworks/community/blogs/jfp/entry/Elementary_Matrix_Operations_In_Python?lang=en

numpy 
2D array and np.dot() to do outer product as matrix.
In Python,  `+ - * /` are element wise for python np.array in general
But not the case np.matrix

https://docs.scipy.org/doc/numpy-dev/user/numpy-for-matlab-users.html
