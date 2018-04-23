How to update an Excel file and publish to PDF.
3 direction:
 - convert Excel to XML (better for text only)
 - convert Excel to HTML(can have charts as pictures and tables)
 - [convert Excel, use VBA to trigger PDF publish](http://www.contextures.com/excelvbapdf.html)</br>
 https://stackoverflow.com/questions/19631511/how-do-i-save-excel-sheet-as-html-in-python



[library xhtml2pdf 0.2.1 for python2](https://pypi.python.org/pypi/xhtml2pdf)

[library weasyprint for python3](http://weasyprint.readthedocs.io/en/latest/tutorial.html)

[js rendering phantomjs](https://gist.github.com/philfreo/44e2e26a65820497db234d0c66ed58ae)

[commercial solutions]
  - https://www.ecrion.com/solutions/document-automation
  - https://www.reportlab.com/
  - https://jugad2.blogspot.ch/2013/06/create-pdf-books-with-xmltopdfbook.html
  - https://www.blog.pythonlibrary.org/2012/07/18/parsing-xml-and-creating-a-pdf-invoice-with-python/



# Access excel with Pandas
https://github.com/jmcnamara/XlsxWriter/blob/master/dev/docs/source/working_with_pandas.rst#accessing-xlsxwriter-from-pandas

# Formatting column
http://xlsxwriter.readthedocs.io/worksheet.html#set_column


```
# from writer access workbook and worksheet
writer = pd.ExcelWriter(outputFile, engine='xlsxwriter')

workbook  = writer.book

# worksheet = writer.sheets['Sheet1']
worksheet = workbook.add_worksheet('Sheet1')
writer.sheets['Sheet1'] = worksheet

# formatting cell,row,column
formatEpoch = workbook.add_format({'num_format': '0'})

worksheet.set_column(1, 1, 30)  # Width of column B set to 30.
worksheet.set_column(1, 1, 30, formatEpoch)  # Width of column B set to 30.
worksheet.set_column('B:B', None, formatEpoch)  # Width of column B set to None.
```

# interpolate timeseries data and save into EXCEL

https://github.com/pandas-dev/pandas/issues/12552<br/>

https://stackoverflow.com/questions/30530001/python-pandas-time-series-interpolation-and-regularization/30530329?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa<br/>

https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.reindex.html<br/>

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

"""
above link solutions:
Based on the existing DataFrame A with column timestamp,
create a resample DataFrame B with good timestamps
concate A and B, sort by timestamps

Best solution can have a same timerage for all signals:
Generate needed timerange and values np.nan  eg. timestamps4, values4
Then save above as a single DataFrame(Used to keep result DataFrame inline) eg. ts4
Concat each signal column by column into a DataFrame eg. ts
Sort the DataFrame by index and interpolate via method='time' and drop_duplicates
Drop the values4 from DataFrame and select results via df.lic[timestamps4]
"""

values = [1000, 2000, 5000, 6000, 7000]
values2 = [10, 20, 50, 60, 70]
values3 = []
values4 = [np.nan]  * 10
timestamps = pd.to_datetime(['2015-01-04 08:30:00',
                             '2015-01-04 08:37:05',
                             '2015-01-04 08:41:07',
                             '2015-01-04 08:43:05',
                             '2015-01-04 08:54:05'])
timestamps2 = pd.to_datetime(['2015-01-04 08:31:00',
                             '2015-01-04 08:36:05',
                             '2015-01-04 08:43:07',
                             '2015-01-04 08:46:05',
                             '2015-01-04 08:54:05'])
timestamps3 = pd.to_datetime([])
timestamps4 = pd.date_range(start='2015-01-04 08:20:00', freq='300S', periods=10 )
# 1420359900000 2015-01-04 08:25:00 point included
# 1420362300000 2015-01-04 09:05:00 point excluded
# newFreq= pd.to_datetime(range(1420359900000, 1420362300000, 300000), unit='ms')

# ts = pd.Series(values, index=timestamps)
# data = {'values': values, 'timestamps': timestamps, 'values2': values2, 'timestamps2': timestamps2}
# ts = pd.DataFrame(data, index=timestamps)

data = {'values': values}
data2 ={'values2': values2}
data3 ={'values3': values3}
data4 ={'values4': values4}

ts = pd.DataFrame(data, index=timestamps)
ts2 = pd.DataFrame(data2, index=timestamps2)
ts3 = pd.DataFrame(data3, index=timestamps3)
ts4 = pd.DataFrame(data4, index=timestamps4)
# Concat column by column First DF avoid empty like here, use ts4 have less issues
ts=pd.concat([ts3,ts2,ts,ts4], axis=1).sort_index()
print ts
# ts
# ts[ts==-1] = np.nan

# # this step is handled by ts4 creation
# newFreq=ts.resample('300S').asfreq()
# print newFreq
# new=pd.concat([ts,newFreq]).sort_index()  # pd.merge, pd.join does not work???
new=ts.sort_index()
print new

new=new.interpolate(method='time')
print new

# get rid of same timestamps with same values
new = new.drop_duplicates()
print new

# drop the columns 'timestamps' inplace
# del new['timestamps']
# drop the columns 'timesamps', keep df not changed
# new.drop(['timesamps'], axis=1)

# get only the good resolution points
# new = new.resample('300S').asfreq()
new = new.loc[timestamps4].drop(['values4'], axis=1)
print new

new = new[new.index < pd.to_datetime(1420361400000, unit='ms')] # < 8:50
new = new[pd.to_datetime(1420360800000,unit='ms') < new.index] # 8:20 <
print new

```

# DataFrame resample
https://www.quantconnect.com/tutorials/introduction-python-pandas-resampling-dataframe/
