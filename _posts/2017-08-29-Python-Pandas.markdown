---
layout: post
title:  "Python Pandas"
date:   2017-08-29 14:51:39 +0100
comments: true  
categories: Python
---




# Table of Contents

- [Initialate an empty DataFrame](#initialate-an-empty-dataframe)
- [Populate DataFrame with data](#populate-dataframe-with-data)
- [Merge DataFrames into one](#merge-dataframes-into-one)
- [Drop duplicates](#drop-duplicates)  
- [write DataFrame into csv or excel file](#write-dataframe-into-csv-file)
- [change nan to None](#change-nan-to-None)
- [select row](#select-row)
- [sort](#sort)








import pandas as pd  

# Initialate an empty DataFrame
    df = pd.DataFrame()  

# Populate DataFrame with data
1. column name is a variable, df is modified

   `df[signalList[index_signal]+ ' timestamp'] = [1, 2, 3, 4, 5]`   

2. column name is a fixed string, df is not modified

    `df.assign(col_xyz=[1, 2, 3, 4, 5])`   

3. new DataFrame with data
    ```
    df = pd.DataFrame({'A':[1,2,3], 'B':[4,5,6]})  
    df2 = pd.DataFrame([1,2,3,4,5], columns=['C'])
    
    # if value is not list but a single scalar or a string, above won't work
    # ValueError: If using all scalar values, you must pass an index
    # add index=[0]
    
    pd.DataFrame({'tag':50}, index=[0])
    pd.DataFrame({'tag':'single'}, index=[0])
    ```
4. assign several columns  
    `df = df.assign(C=df['A']**2, D=df.B*2)`   

5. add more data into DataFrame [doc](https://pandas.pydata.org/pandas-docs/stable/indexing.html#selection-by-label)
    ```
    new_row_lable = pd.DataFrame.index.max() + 1
    pd.DataFrame.loc[new_row_label] = [val1, val2, val3]
    ```

# Merge DataFrames into one
1. only for DataFrame, can't merge with Series.
2. allow DataFrame has different number of rows
3. merge left side Df and right side Df, not like concat up and down  
`pd.merge(left=df, right=df2, left_index=True, right_index=True, how='outer')`

concat DataFrames A and B in axis columns  
```
pd.concat([df_data, df_new_data], axis=1)
```


# Drop duplicates   
  drop a row
  ```
  pd.DataFrame.drop([row_label_1, row_label_2])
  ```

  drop a row where Nan exist in columns
  ```
  pd.DataFrame.dropna(subset=[col1, col2])
  ```

  drop columns with same column name
  ```
  pd.DataFrame.loc([:, ~pd.DataFrame.columns.duplicate()])
  ```

  drop columns with no name in header
  ```
  del df_new_data['Unnamed: 0']
  ```



# Write DataFrame into csv or excel file

    ```
    import os
    # if file does not exist write header 
    if not os.path.isfile('filename.csv'):
        df.to_csv('filename.csv',header ='column_names')
    else: # else it exists so append without writing the header
        df.to_csv('filename.csv',mode = 'a',header=False)
    ```

[excel writing ](http://xlsxwriter.readthedocs.io/working_with_pandas.html)
this a very good website details writing options to excel
```
writer = pd.ExcelWriter("pandas_datetime.xlsx",
                        engine='xlsxwriter',
                        datetime_format='mmm d yyyy hh:mm:ss',
                        date_format='mmmm dd yyyy')

workbook  = writer.book
worksheet = writer.sheets['Sheet1']

# Add some cell formats.
format1 = workbook.add_format({'num_format': '#,##0.00'})
format2 = workbook.add_format({'num_format': '0%'})

# Set the column width and format.
worksheet.set_column('B:B', 18, format1)

# Set the format but not the column width.
worksheet.set_column('C:C', None, format2)

df4 = pd.DataFrame()
df4.to_excel(writer, sheet_name='Sheet1',
             startrow=7, startcol=4, header=False, index=False)
```

```

```

# Change nan to None 
  [links](https://stackoverflow.com/questions/14162723/replacing-pandas-or-numpy-nan-with-a-none-to-use-with-mysqldb)
  ```
  >>> x = np.array([1, np.nan, 3])
  >>> y = np.where(np.isnan(x), None, x)
  >>> print y
  [1.0 None 3.0]
  >>> print type(y[1])
  <type 'NoneType'>
  ```
  
  NaN in Pandas is not [None, null, nan], it's a special type. Avoid using NaN==None or if NaN.
  ```
  pd.DataFrame.isnull()
  ```
  
  ```
  import json
  import pandas as pd
  import numpy as np
  
  dataJson = json.loads('{"tags":[{"name":"datamissing","datapoints":[[1481479200000,0,3],[1481482800000,null,3]]}]}')
  df = pd.DataFrame(dataJson['tags'][0]['datapoints'])
  print df
	None
		       0    1  2
	0  1481479200000  0.0  3
	1  1481482800000  NaN  3
  values = df[1]
  print np.where(np.isnan(values),None,values)
  	[0.0 None]
  ```
  
  https://www.oreilly.com/learning/handling-missing-data
  
  https://chartio.com/resources/tutorials/how-to-check-if-any-value-is-nan-in-a-pandas-dataframe/
  
    
# select row
To select rows whose column value equals a scalar, some_value, use ==:
```
df.loc[df['column_name'] == some_value]
```
To select rows whose column value is in an iterable, some_values, use isin:
```
df.loc[df['column_name'].isin(some_values)]
```
Combine multiple conditions with &:
```
df.loc[(df['column_name'] == some_value) & df['other_column'].isin(some_values)]
```
https://stackoverflow.com/questions/17071871/select-rows-from-a-dataframe-based-on-values-in-a-column-in-pandas

example:
```
import pandas as pd
import numpy as np
df = pd.DataFrame({'A': 'foo bar foo bar foo bar foo foo'.split(),
                   'B': 'one one two three two two one three'.split(),
                   'C': np.arange(8), 'D': np.arange(8) * 2})
print(df)
#      A      B  C   D
# 0  foo    one  0   0
# 1  bar    one  1   2
# 2  foo    two  2   4
# 3  bar  three  3   6
# 4  foo    two  4   8
# 5  bar    two  5  10
# 6  foo    one  6  12
# 7  foo  three  7  14

print(df.loc[df['A'] == 'foo'])
```
Note, however, that if you wish to do this many times, it is more efficient to make an index first, and then use df.loc:
```
df = df.set_index(['B'])
print(df.loc['one'])
```
yields
```
       A  C   D
B              
one  foo  0   0
one  bar  1   2
one  foo  6  12
```
or, to include multiple values from the index use df.index.isin:
```
df.loc[df.index.isin(['one','two'])]
```
yields
```
       A  C   D
B              
one  foo  0   0
one  bar  1   2
two  foo  2   4
two  foo  4   8
two  bar  5  10
one  foo  6  12
```
	
In fact, df[df['colume_name']==some_value] also works


# sort
in case we need to sort based on values inside one or two columns, `df.sort_values(by=['col1','col2'], ascending=[True, False])` is very handy.
https://www.w3resource.com/python-exercises/pandas/python-pandas-data-frame-exercise-16.php

in case we need to re-organize the DataFrame index(row) names `df.sort_index(axis=0, ascending=True)`;
or to re-organize the DataFrame column names `df.sort_index(axis=1, ascending=False)` can do the work.
https://stackoverflow.com/questions/11067027/python-pandas-re-ordering-columns-in-a-dataframe-based-on-column-name

