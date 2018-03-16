Python2
https://www.saltycrane.com/blog/2008/04/how-to-use-pythons-enumerate-and-zip-to/
```
from itertools import izip, count
alist = ['a1', 'a2', 'a3']
blist = ['b1', 'b2', 'b3']

for i, a, b in izip(count(), alist, blist):
    print i, a, b
```
yields the exact same result as above, but is faster and uses less memory.


#2 Jeremy Lewis commented on 2009-05-29:
```
>>> def foo():
...  for i, x, y in izip(count(), a, b):
...   pass
...
>>> def bar():
...  for i, (x, y) in enumerate(zip(a, b)):
...   pass
...
>>> delta(foo)
0.0213768482208
>>> delta(bar)
0.180979013443
```

where a = b = xrange(100000) and delta(f(x)) denotes the runtime in seconds of f(x).


Python3
https://docs.python.org/3.6/library/itertools.html
`zip_longest()` is used, no more izip() found in doc.
