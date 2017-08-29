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
- [write DataFrame into csv file](#write-dataframe-into-csv-file)







`import pandas as pd
`

# Initialate an empty DataFrame
    `df = pd.DataFrame()
    `

# Populate DataFrame with data
1. column name is a variable, df is modified

   `df[signalList[index_signal]+ ' timestamp'] = [1, 2, 3, 4, 5]
   `

2. column name is a fixed string, df is not modified

    `df.assign(col_xyz=[1, 2, 3, 4, 5])
    `

3. new DataFrame with data
    ```
    df = pd.DataFrame({'A':[1,2,3], 'B':[4,5,6]})  
    df2 = pd.DataFrame([1,2,3,4,5], columns=['C'])
    ```
4. assign several columns  
    `df = df.assign(C=df['A']**2, D=df.B*2)
    `


# Merge DataFrames into one
1. only for DataFrame, can't merge with Series.
2. allow DataFrame has different number of rows
3. merge left side Df and right side Df, not like concat up and down  
`pd.merge(left=df, right=df2, left_index=True, right_index=True, how='outer')
`


# write DataFrame into csv file
    ```
    import os
    # if file does not exist write header 
    if not os.path.isfile('filename.csv'):
        df.to_csv('filename.csv',header ='column_names')
    else: # else it exists so append without writing the header
        df.to_csv('filename.csv',mode = 'a',header=False)
    ```
