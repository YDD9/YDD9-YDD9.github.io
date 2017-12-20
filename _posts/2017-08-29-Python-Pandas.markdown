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
- [write DataFrame into csv file](#write-dataframe-into-csv-file)
- [change nan to None](#change-nan-to-None)








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



# Write DataFrame into csv file
    ```
    import os
    # if file does not exist write header 
    if not os.path.isfile('filename.csv'):
        df.to_csv('filename.csv',header ='column_names')
    else: # else it exists so append without writing the header
        df.to_csv('filename.csv',mode = 'a',header=False)
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
  
  Nan in Pandas is not [None, null, nan], it's a special type. Avoid using Nan==None or if Nan.
  ```
  pd.DataFrame.isnull()
  ```
  
    
