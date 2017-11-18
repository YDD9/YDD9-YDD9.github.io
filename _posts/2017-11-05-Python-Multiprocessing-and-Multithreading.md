---
layout: post
title:  "Python Multiprocessing and Multithreading"
date:   2017-11-05 14:58:39 +0100
comments: true  
categories: Python
---

[credit given to blog](https://www.toptal.com/python/beginners-guide-to-concurrency-and-parallelism-in-python)


# Download 10000 pictures quickly

### Fake parallele threading
Using lib threading, you can release the GIL and continues to the next download when previous download is on-going.
It's not real parallele download 100 pictures at the same time, but it's much better than sequencial downloading.
Implementation is a little bit tricky compared to multiprocessing methode.

### Multiprocessing queue pool or process
Using lib multiprocessing, make usage of several CPUs to run the downloading in the processing queue **pool**.
Implementation is simple, but cost of creating a new process is not cheap and can have overhead load issues.   
[**process**](https://pymotw.com/2/multiprocessing/basics.html)


### Use IronPython or Jython
Python with GIL issues is implemented by CPython. Python implementaion with .net or java don't have GIL issue,
so you can carefully choose to work with IronPython or Jython, but be aware of compability issues with Django, etc...

### Cached queue and download executed on multiple PCs 
Using lib Python RedisQueue or Celery to queue in redis or other DB, then execute download from several PCs.
Working with Cloud, this solution should be nature. 


inside the example code, there is an interesting function functools.partial. It is used to re-define your original function to a bew function by changing input variable of the original function. example is **[here](https://www.pydanny.com/python-partials-are-fun.html)**


Python 3.5+ has a new lib **asyncio**, it's the new asynchrous module for python, here is comparison [article](http://masnun.rocks/2016/10/06/async-python-the-different-forms-of-concurrency/) and a [tutorial](http://stackabuse.com/python-async-await-tutorial/) for asyncio
