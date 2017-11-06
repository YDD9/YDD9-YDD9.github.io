---
layout: post
title:  "Python Time Performance"
date:   2017-11-07 00:17:39 +0100
comments: true  
categories: Python
---


[stackoverflow memorization improves performance for overlapped recursive in Python ?](https://stackoverflow.com/questions/38783043/does-python-memorization-improve-performance-of-recursive-functions)

**Memorization will not improve performance of recursive functions unless you are going to use the function multiple times - then the previously calculated results will be returned immediately. That would make the performance better than linear too**

Try this out yourself
```
import timeit

setup ='''
def fib3(n): #FASTEST YET
    fibs= [0,1] #list from bottom up
    for i in range(2, n+1):
        fibs.append(fibs[-1]+fibs[-2])
    return fibs[n]

def fib4(n, computed= {0:0, 1:1}): #memoization
    if n not in computed:
        computed[n]= fib4(n-1, computed)+ fib4(n-2, computed)
    return computed[n]

'''

print (min(timeit.Timer('fib3(600)', setup=setup).repeat(3, 1)))

print (min(timeit.Timer('fib4(600)', setup=setup).repeat(3, 1)))
# This will show fib4 taking longer.
# Result of single run, recursion is costly.

0.00010111480978441638
0.00039419570581368307
[Finished in 0.1s]
```  
If you change the last two lines, that is repeat each for 100 times, the result changes Now fib4 becomes faster, as if not only no recursion, almost like no additional time to compute at all
```
print (min(timeit.Timer('fib3(600)', setup=setup).repeat(3, 100)))
print (min(timeit.Timer('fib4(600)', setup=setup).repeat(3, 100)))
```

**Results with 50 repeats, linear function is now growing linearly, but memorization takes almost no additional time!**
```
0.00501430622506104
0.00045805769094068097
[Finished in 0.1s]
```

**Results with 100 repeats, linear function takes more than double time, but memorization takes almost little additional time**
```
0.01583016969421893
0.0006815746388851851
[Finished in 0.2s]
```
