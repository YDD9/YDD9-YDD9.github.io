---  
layout: post  
title:  "Test Driven Development Python"  
date:   2017-02-14 11:49:39 +0100  
categories: Python backend and web test automation  
---  

# Table of contents  
1. [Environment setup](#setup)  



## Environment setup <a name=setup></a>
use conda to install a new env python3.5 anaconda  
use conda to install selenium 3.0.2  
use conda to install django  
[download firefox remote driver for your MAC, Win](https://developer.mozilla.org/en-US/docs/Mozilla/QA/Marionette/WebDriver)  
[extracted the executable to python3.5 path](http://stackoverflow.com/questions/40208051/selenium-using-python-geckodriver-executable-needs-to-be-in-path)  
ex:   
[...]\AppData\Local\Continuum\Anaconda2\envs\python35  this path already exists as environment path if not add it to Path.  
install VScode and Python extension by Don Jayamanne, change to your pythonPath from launch.json when debug.

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python",
            "type": "python",
            "request": "launch",
            "stopOnEntry": true,
            "pythonPath": "[...]/AppData/Local/Continuum/Anaconda2/envs/python35/python",
            "program": "${file}",
            "cwd": "${workspaceRoot}",
            "debugOptions": [
                "WaitOnAbnormalExit",
                "WaitOnNormalExit",
                "RedirectOutput"
            ]
        },
        [...]
```

create a django server `django-admin.py startproject superlists`  
bring up the server `python3 manage.py runserver`

open another cmd to run functional_test.py

```
from selenium import webdriver

browser = webdriver.Firefox()
browser.get('http://localhost:8000')
assert 'Welcome to Django' in browser.title

# browser.get('https://www.google.com/')
# assert 'Welcome To Zscaler Directory Authentication' in browser.title

print(browser.title)
```

ready to go!



