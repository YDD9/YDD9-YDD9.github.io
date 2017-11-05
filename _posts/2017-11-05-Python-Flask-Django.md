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
```

Advanced usage material to **read(https://www.codementor.io/sheena/advanced-use-python-decorators-class-function-du107nxsv)**
