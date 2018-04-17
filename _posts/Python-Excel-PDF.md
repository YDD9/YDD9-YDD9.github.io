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
