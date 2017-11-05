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

### Multiprocessing queue pool
Using lib multiprocessing, make usage of several CPUs to run the downloading in the processing que pool.
Implementation is simple, but cost of creating a new process is not cheap and can have overhead load issues.

### Use IronPython or Jython
Python with GIL issues is implemented by CPython. Python implementaion with .net or java don't have GIL issue,
so you can carefully choose to work with IronPython or Jython, but be aware of compability issues with Django, etc...

### Cached queue and download executed on multiple PCs 
Using lib Python RedisQueue or Celery to queue in redis or other DB, then execute download from several PCs.
Working with Cloud, this solution should be nature. 


inside the example code, there is an interesting function functools.partial. It re-define your original function with another name by changing the orig function input variable. example is **[here](https://www.pydanny.com/python-partials-are-fun.html)**
