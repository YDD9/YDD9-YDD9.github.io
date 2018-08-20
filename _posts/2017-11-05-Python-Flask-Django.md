---
layout: post
title:  "Python Flask Django"
date:   2017-11-05 15:58:39 +0100
comments: true  
categories: Python
---

Flask introduces the @ decorator, it wraps your function to a new one so that new function can be called by http requests and at the same time it does exactly as it meant to do, a very good detailed explanation is **[here](https://www.codementor.io/sheena/introduction-to-decorators-du107vo5c)**
```
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run(
        host='0.0.0.0',
        port=80,
        debug=False)
```

Advanced usage material to **[read](https://www.codementor.io/sheena/advanced-use-python-decorators-class-function-du107nxsv)**


Another example of usage case of decorator. It solves the issues of recursive limit. **[Read link](http://code.activestate.com/recipes/474088/)**
```
def tail_call_optimized(g):
  """
  This function decorates a function with tail call
  optimization. It does this by throwing an exception
  if it is it's own grandparent, and catching such
  exceptions to fake the tail call optimization.
  
  This function fails if the decorated
  function recurses in a non-tail context.
  """
  def func(*args, **kwargs):
    f = sys._getframe()
    if f.f_back and f.f_back.f_back \
        and f.f_back.f_back.f_code == f.f_code:
      raise TailRecurseException(args, kwargs)
    else:
      while 1:
        try:
          return g(*args, **kwargs)
        except TailRecurseException, e:
          args = e.args
          kwargs = e.kwargs
  func.__doc__ = g.__doc__
  return func

@tail_call_optimized
def factorial(n, acc=1):
  "calculate a factorial"
  if n == 0:
    return acc
  return factorial(n-1, n*acc)
```

# encode

## percent-encoding used in html, url, restAPI payload
https://www.obkb.com/dcljr/chars.html

`%5D` % following by two digits HEX number
```
>>> import urllib
>>> urllib.quote('5vxK]Ek1K^6%')
'5vxK%5DEk1K%5E6%25'
```

## string decode
```
>>> '5D'.decode('HEX')
']'
```

## uni code utf-8
http://pgbovine.net/unicode-python.htm
Now forget everything you've learned about ASCII strings. We'll now learn about Unicode strings, which can represent any possible character in any language, not just ASCII characters that appear directly on your keyboard. If you want to develop software that works internationally (especially on the Web), then you need to understand Unicode.
```
>>> unicode('\x80abc', errors='replace')
u'\ufffdabc'
>>> unicode('\x80abc', errors='ignore')
u'abc'
```

# JSON Web Tokens (JWT) with flask

https://codeburst.io/jwt-authorization-in-flask-c63c1acf4eeb
