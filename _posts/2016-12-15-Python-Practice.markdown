---
layout: post
title:  "Python Practice"
date:   2016-12-15 14:08:39 +0100
categories: Python
---

# Table of contents  
[Date time](#datetime)  
[Division](#division)  
[Function arguments](#argparser)  
[Python installer](#pythoninstaller)  
[function \_\_init\_\_](#init)  
[function lambda](#lambda)  

## Date time <a name="datetime"></a>

```
from datetime import datetime  
import time  
import pandas as pd
import calendar
```  

Run have an idea of computation time, `time.time()` is useful.
Conversion between epochtime, structure time and string I use below function to avoid issues of daylight saving, timezone.
![My helpful screenshot]({{ site.url }}/images/Datetime.jpg)

<style>
table{
    border-collapse: collapse;
    border-spacing: 0;
    border:2px solid #ff0000;
}

th{
    border:2px solid #000000;
}

td{
    border:1px solid #000000;
}
</style>


|formatter    | description                           |
| ------------|---------------------------------------|
|"%a"		|Locale’s abbreviated weekday name.		| 
|"%A"		|Locale’s full weekday name.		| 
|"%b"		|Locale’s abbreviated month name.		| 
|"%B"		|Locale’s full month name.		| 
|"%c"		|Locale’s appropriate date and time representation.		| 
|"%d"		|Day of the month as a decimal number [01,31].		| 
|"%H"		|Hour (24-hour clock) as a decimal number [00,23].		| 
|"%I"		|Hour (12-hour clock) as a decimal number [01,12].		| 
|"%j"		|Day of the year as a decimal number [001,366].		| 
|"%m"		|Month as a decimal number [01,12].		| 
|"%M"		|Minute as a decimal number [00,59].		| 
|"%p"		|Locale’s equivalent of either AM or PM.		|
|"%S"		|Second as a decimal number [00,61].		|
|"%U"		|Week number of the year (Sunday as the first day of the week) as a decimal number [00,53]. All days in a new year preceding the first Sunday are considered to be in week 0.		|
|"%w"		|Weekday as a decimal number [0(Sunday),6].		| 
|"%W"		|Week number of the year (Monday as the first day of the week) as a decimal number [00,53]. All days in a new year preceding the first Monday are considered to be in week 0.		|
|"%x"		|Locale’s appropriate date representation.		| 
|"%X"		|Locale’s appropriate time representation.		| 
|"%y"		|Year without century as a decimal number [00,99].		| 
|"%Y"		|Year with century as a decimal number.		| 
|"%Z"		|Time zone name (no characters if no time zone exists).		| 
|"%%"		|A literal '%' character.		|

HTML example
http://www.w3schools.com/html/html_tables.asp

Generate a list of timestamp  

```
rng = pd.date_range('2016-08-01', periods= 10, freq='12H')
rng.strftime('%Y-%m-%d')   
array(['2016-08-01', '2016-08-01', '2016-08-02', '2016-08-02',  
       '2016-08-03', '2016-08-03', '2016-08-04', '2016-08-04',  
       '2016-08-05', '2016-08-05'],  
      dtype='|S10')  
```  
 
Check leap year:  

```
calendar.isleap(datetime.now().timetuple().tm_year)
```  

## Division <a name="division"></a>  
By default, python2.7 division return only the rounded integer part. To force results into float try below

```
>>> from __future__ import division
>>> 2/3
>>> 0.6666666666666666
```  


## Function arguments <a name="argparser"></a>  
two common one  

```
import argparser
import getopt
```  

An example using only Unix style options:  

```
>>> import getopt
>>> args = '-a -b -cfoo -d bar a1 a2'.split()
>>> args
['-a', '-b', '-cfoo', '-d', 'bar', 'a1', 'a2']
>>> optlist, args = getopt.getopt(args, 'abc:d:')
>>> optlist
[('-a', ''), ('-b', ''), ('-c', 'foo'), ('-d', 'bar')]
>>> args
['a1', 'a2']
```    

Using long option names is equally easy:

```
>>> s = '--condition=foo --testing --output-file abc.def -x a1 a2'
>>> args = s.split()
>>> args
['--condition=foo', '--testing', '--output-file', 'abc.def', '-x', 'a1', 'a2']
>>> optlist, args = getopt.getopt(args, [
...     'condition=', 'output-file=', 'testing'])
>>> optlist
[('--condition', 'foo'), ('--testing', ''), ('--output-file', 'abc.def')]
>>> args
['a1', 'a2']

# "--help" and "-h" style combined:
>>> s = '--condition=foo --testing --output-file abc.def -x a1 a2'
>>> args = s.split()
>>> args
['--condition=foo', '--testing', '--output-file', 'abc.def', '-x', 'a1', 'a2']
>>> optlist, args = getopt.getopt(args, 'x', [
...     'condition=', 'output-file=', 'testing'])
>>> optlist
[('--condition', 'foo'), ('--testing', ''), ('--output-file', 'abc.def'), ('-x', '')]
>>> args
['a1', 'a2']

```    

In a script, typical usage is something like this:

```
import getopt, sys

def main():
    try:
        opts, args = getopt.getopt(sys.argv[1:], "ho:v", ["help", "output="])
    except getopt.GetoptError as err:
        # print help information and exit:
        print str(err)  # will print something like "option -a not recognized"
        usage()
        sys.exit(2)
    output = None
    verbose = False
    for o, a in opts:
        if o == "-v":
            verbose = True
        elif o in ("-h", "--help"):
            usage()
            sys.exit()
        elif o in ("-o", "--output"):
            output = a
        else:
            assert False, "unhandled option"
    # ...

if __name__ == "__main__":
    main()
```   

Note that an equivalent command line interface could be produced with less code and more informative help and error messages by using the argparse module:

```
import argparse

if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument('-o', '--output')
    parser.add_argument('-v', dest='verbose', action='store_true')
    args = parser.parse_args()
    # ... do something with args.output ...
    # ... do something with args.verbose ..    
```  

Combining Positional and Optional arguments  
Define firstly all positional arguments then start optional arguments.      
You can have help / extra information and display them.   
You can force argument type to int instead of default string.    
You can provide a default value for the argument.    

```
import argparse
parser = argparse.ArgumentParser(description='A foo that bars',
                                epilog="And that's how you'd foo a bar")  
parser.print_help()                   
parser.add_argument("square", type=int,  
                    help="display a square of a given number")
parser.add_argument("-v", "--verbose", action="store_true",  
                    help="increase output verbosity")  
parser.add_argument('--example', nargs='?',    
                    const=1, type=int)                     
args = parser.parse_args()  
answer = args.square**2  
if args.verbose:  
    print "the square of {} equals {}".format(args.square, answer)  
else:  
    print answer  
    
print args
print args.example
print args.info
```   

nargs='?' means 0-or-1 arguments  
const=1 sets the default when there are 0 arguments  
type=int converts the argument to int  





