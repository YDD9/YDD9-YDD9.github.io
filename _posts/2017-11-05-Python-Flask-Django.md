---
layout: post
title:  "Python Flask Django"
date:   2017-11-05 15:58:39 +0100
comments: true  
categories: Python
---

Flask introduce the @ decorator into Python, it just save you to type, details usage is **[here]()**
```
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"
```
