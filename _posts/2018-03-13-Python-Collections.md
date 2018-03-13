---
layout: post
title:  "Python Collections"
date:   2018-03-13 11:58:39 +0100
comments: true  
categories: Python collections magic functions
---



# [collections module in Python](https://docs.python.org/2/library/collections.html)

This module implements specialized container datatypes providing alternatives to Python’s general purpose built-in containers, dict, list, set, and tuple.

namedtuple()	factory function for creating tuple subclasses with named fields

deque	list-like container with fast appends and pops on either end

Counter	dict subclass for counting hashable objects

OrderedDict	dict subclass that remembers the order entries were added

defaultdict	dict subclass that calls a factory function to supply missing values

In addition to the concrete container classes, **the collections module provides abstract base classes that can be used to test whether a class provides a particular interface**, for example, whether it is hashable or a mapping.

This means what exactly?? Here's a good article explains in more details.
[Using collections in Python](https://medium.com/bynder-tech/using-collections-in-python-36129737b5a1)
```
class Event(dict):
    def __getitem__(self, key):
        value = super().__getitem__(key)
        return f"[{self.__class__.__name__}]: {value}"

    def log_event_type(event_type, date, **kwargs):
        print(f"{date} - {event_type}")
```
side note about magic function `__getitem__`:</br>
http://www.diveintopython.net/object_oriented_framework/special_class_methods.html</br>

side note about magic function `__class__`:</br> 
https://stackoverflow.com/questions/20599375/what-is-the-purpose-of-checking-self-class-python</br>

side note about magic function `__name__`:</br> 
https://stackoverflow.com/questions/419163/what-does-if-name-main-do</br>
`__name__ is a built-in variable which evaluates to the name of the current module. However, if a module is being run directly (as in myscript.py above), then __name__ instead is set to the string "__main__"`


If we try to use this code, we’ll find that it covers the basic cases as we might expect:
```
event = Event(user="user", event_type="login", date="2017-07-11")

print(event['user'])  # [Event]: user
log_event_type(**event)   # 2017-07-11 - login
```

Here we would expect the values with the transformation we defined (including the class of the event as prefix),
but instead we get the underlying values of the dictionary, ignoring our implementation of __getitem__() . This code will print the following:
```
for key, value in event.items():
    print(f"{key} = {value}")

# user = user
# event_type = login
# date = 2017-07-11
```

All of these problems are solved, if instead we subclass `collections.UserDict`. It’s available with the goal of creating dictionary-like objects in our code.
```
class Event(collections.UserDict):
    def __getitem__(self, key):
        value = super().__getitem__(key)
        return f"[{self.__class__.__name__}]: {value}"
```
