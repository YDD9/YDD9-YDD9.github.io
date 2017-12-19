
grep can be used together with regex, put regex inside single quote.   
https://www.cyberciti.biz/faq/searching-multiple-words-string-using-grep/    

```
grep 'word1\|word2\|word3' /path/to/file
grep '^redis*'
```

awk can be used to filter as well
https://www.cyberciti.biz/faq/bash-scripting-using-awk/   
```
awk '{ print }' /etc/passwd

awk '{ print $0 }' /etc/passwd
# use ':' to seperate and print the first field
awk -F':' '{ print $1 }' /etc/passwd

# Pattern Matching
# You can only print line of the file if pattern matched. 
For e.g. display all lines from Apache log file if HTTP error code is 500 
(9th field logs status error code for each http request):
awk '$9 == 500 { print $0}' /var/log/httpd/access.log
```
