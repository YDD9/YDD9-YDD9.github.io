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
|"%p"		|Locale’s equivalent of either AM or PM.		|(1)
|"%S"		|Second as a decimal number [00,61].		|(2)
|"%U"		|Week number of the year (Sunday as the first day of the week) as a decimal number [00,53]. All days in a new year preceding the first Sunday are considered to be in week 0.		|(3)
|"%w"		|Weekday as a decimal number [0(Sunday),6].		| 
|"%W"		|Week number of the year (Monday as the first day of the week) as a decimal number [00,53]. All days in a new year preceding the first Monday are considered to be in week 0.		|(3)
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


