---
layout: post
title:  "Cloud Foundry Predix"
date:   2017-08-06 14:48:39 +0100
comments: true  
categories: Cloud Foundry Predix
---


- [Table of Contents](#table-of-contents)
- [cf-uaac installation](#cf-uaac-installation)
- [cf-uaac troubleshoot](#cf-uaac-troubleshoot)   
- [clean up space](#clean-up-space)   
- [push Python app in cloud with specific buildpack and Python version](#push-python-app-in-cloud-with-specific-buildpack-and-python-version)
- [Python app in cloud with Flask](#python-app-in-cloud-with-flask)
- [Python app with own libraries](#python-app-with-own-libraries)
- [Python app with multiple .py files but only one as entry point](#python-app-with-multiple-py-files-but-only-one-as-entry-point)
- [Python app in cloud for different space](#python-app-in-cloud-for-different-space)
- [Push Python app in Predix-Analytics-Framework CI/CD](#push-python-app-in-predix-analytics-framework-cicd)
- [Access Predix Postgres service from local PgAdmin](#Access-Predix-Postgres-service-from-local-PgAdmin)


# cf-uaac installation

install cf-uaac on win10 is not forward straight, especially when there's certain version of Ruby that doesn't work well.

Basically you have to follow this wiki https://github.com/cloudfoundry/cf-uaac/wiki to install Ruby2.3.x, Ruby-devkit-mingw64/86-x and cf-uaac.

Welcome to the cf-uaac wiki!

Installation:

There has not been much information about what needs to be installed before you run `gem install cf-uaac`, so here are some pre-reqs.

Windows:

Download and install RubyGems (http://rubyinstaller.org/) (Download the installer AND the devkit)
Installer: Run the installer. Make sure you tick all of the boxes regarding the PATH and stuff.
DevKit: I would create a folder at C:/ruby-devkit. Extract the self-extracting file to that location.
Open CMD (or whatever) and cd to your devkit location. Install it (“ruby dk.rb init” and “ruby dk.rb install”)
Install cf-uaac (“gem install cf-uaac”)




Version 2.4.x Ruby doesn't work when you do `ruby install cf-uaac` could be similar issues as https://github.com/rubygems/rubygems/issues/977

Predix also use cf-uaac.

To uninstall `gem uninstall cf-uaac`, to install specific version `gem install cf-uaac -v 3.4.0` [link](https://rubygems.org/gems/cf-uaac/versions/3.4.0)  



# cf-uaac troubleshoot

When executing command `uaac target https://*.predix-uaa.run.asv-pr.ice.predix.io`, error "Use '--skip-ssl-validation' to continue with an insecure target displays" occurs, the work around is set certificate before running scripts 
```
set CERT_FILE_PATH="C:\Users\YDD9\Downloads\cmder\vendor\git-for-windows\usr\ssl\certs\ca-bundle.crt"

# For Mac users
export SSL_CERT_FILE="/Users/YDD9/Downloads/cmder/vendor/git-for-windows/usr/ssl/certs/ca-bundle.crt"

```   

Further reading materials [Predix forum solution](https://forum.predix.io/questions/11900/solution-uaac-target-failed-to-access-invalid-ssl.html) and [Predix advice for java](https://forum.predix.io/articles/11434/ssl-certificate-problem-1.html)  

The best workaround I found is proposed by [cloudfoundry doc](https://discuss.pivotal.io/hc/en-us/articles/115009446348-Deploying-Spring-Cloud-Services-Broker-fails-with-UAAC-target-error):
```
uaac target --ca-cert "C:\Users\YDD9\Downloads\cmder\vendor\git-for-windows\usr\ssl\certs\ca-bundle.crt" https://*.predix-uaa.run.asv-pr.ice.predix.io
```
  
The idea is to find the path of your ca-bundle.crt file(normally in your gitbash path or similar bash path) and force UAAC command to use it.  
  
  
# clean up space  
Delete all apps in a space<br/>
```
for i in $(cf a | tail -n+5 | awk '{print $1}'); do cf delete ${i} -f; done
```

Delete all services in a space<br/>
```
for i in $(cf s | tail -n+5 | awk '{print $1}'); do cf delete-service ${i} -f; done
```

Delete all routes in a space<br/>
```
cf routes | tail -n+4 | while read a b c; do cf delete-route "$c" --hostname "$b" -f; done
# or
cf routes | tail -n+4 | awk '{system("cf delete-route " $3" --hostname "$2" -f")}'

# or to be tested
# https://www.howtoforge.com/tutorial/linux-xargs-command/
cf routes | tail -n+4 | awk '{print $3" --hostname "$2" -f"}' | xargs -n 4 cf delete-route 
```

# push Python app in cloud with specific buildpack and Python version
 1. Check available buildpacks `cf buildpacks`  
    ```
    λ cf buildpacks
    Getting buildpacks...

    buildpack                    position   enabled   locked   filename
    custom_node_buildpack        1          true      false
    java_buildpack               2          true      false    java-buildpack-v3.6.zip
    ruby_buildpack               3          true      false    ruby_buildpack-cached-v1.6.34.zip
    nodejs_buildpack             4          true      false    nodejs_buildpack-cached-v1.5.29.zip
    python_buildpack             5          true      false    python_buildpack-cached-v1.5.15.zip
    predix_openresty_buildpack   6          true      false    staticfile_buildpack-cached-v1.2.1.zip
    php_buildpack                7          true      false    php_buildpack-cached-v4.3.27.zip
    staticfile_buildpack         8          true      false    staticfile_buildpack-cached-v1.3.17.zip
    go_buildpack                 9          true      false    go_buildpack-cached-v1.7.18.zip
    java_buildpack_large_heap    10         true      false    java_buildpack_large_heap.zip
    matlab_buildpack_r2011b      11         true      false    matlab-buildpack-r2011b.zip
    matlab_buildpack_r2012a      12         true      false    matlab-buildpack-r2012a.zip
    matlab_buildpack_r2015a      13         true      false    matlab-buildpack-r2015a.zip
    binary_buildpack             14         true      false    binary_buildpack-cached-v1.0.9.zip
    java-buildpack               15         true      false    java-buildpack-v3.13.zip
    java-offline-buildpack       16         true      false    java-buildpack-offline-v3.13.zip
    python_minconda_buildpack    17         false     false    python_minconda_buildpack.zip
    dotnet_core_buildpack        18         true      false    dotnet-core_buildpack-cached-v1.0.11.zip
    ```
 2. Push app with available python_buildpack or a specific buildpack in github https://forum.predix.io/articles/24714/how-to-specify-a-python-buildpack.html
    ```
    cf push -b python_buildpack
    
    # or specific version of buildpack
    cf push -b https://github.com/cloudfoundry/python-buildpack.git#v1.5.20

    # or specify buildpack in your manifest.yml
    ---
    applications:
    - name: latest-buildpack-demo
    buildpack: https://github.com/cloudfoundry/buildpack-python.git#v1.5.20
    ```
3. Using specific Python version https://forum.predix.io/articles/24718/how-to-specify-a-python-version-for-your-app.html   
   To avoid surprise, you should use the same version of Python in development and in cloud. You need to have the corresponding buildpack then create a `runtime.txt` file in the same directory as `manifest.yml`.  
   Examples of runtime.txt
   ```
    python-3.6.1

   ```
   Examples of runtime.txt
   ```
    python-2.7.13
   ```

# Python app in cloud with Flask
http://codehandbook.org/python-flask-web-application-on-ge-predix/  
https://forum.predix.io/articles/24970/how-to-deploy-a-python-app-on-predix-in-under-10-m.html

# Python app with own libraries
https://forum.predix.io/articles/24733/how-to-include-your-own-private-python-libraries-i.html

# Python app with multiple .py files but only one as entry point
https://forum.predix.io/articles/28026/how-to-deploy-a-python-app-on-predix-consisting-of.html  
```
|-- analytics
  |-- main.py
  |-- commonHelper.py
  |-- historyCalc.py
  |-- futureCalc.py
  |-- manifest.yml
  |-- requirements.txt
  |-- run.sh
```
If your analytics contains multiple .py files and only main.py is the entry point, you can create a __run.sh__ file specify the entry.
```
echo "----------start of analytics----------"
python main.py
```
For requirements.txt you specify all modules used:
```
Flask
requests
pandas
```


# Python app in cloud for different space
https://forum.predix.io/articles/27281/-how-to-deploy-application-on-multiple-spaces-deve.html
```
cf target -s <myspace>
cf push -f <manifest.yml for myspace> -p <dist for myspace>
```
# Push Python app in Predix-Analytics-Framework CI/CD
https://forum.predix.io/articles/25018/how-to-analytics-framework-workflow-automation-usi.html


# Access Predix Postgres service from local PgAdmin
Below example is done on postgres, the same should be applicable for all similar services.
https://github.com/briangann/predix-chisel-postgres

Install chisel inside your space and linked to Postgres
```
#create predix postgres instance
cf create-service <postgres-db-service-name> <my-db-plan> <my-db-name>
#for example : cf create-service postgres shared-nr postgres-chisel

#clone repo & deploy the app
git clone https://github.com/briangann/predix-chisel-postgres.git
cd predix-chisel-postgres/app/
cf push --no-start

#start and bind chisel service to your existing Postgres database instance
cf start predix-chisel-postgres
cf bind-service <postgres-db-service-name> <my-db-name>
#for example : cf bind-service predix-chisel-postgres postgres-chisel
cf restage predix-chisel-postgres
```
download [chisel](https://github.com/jpillora/chisel) and run it as client, remove `--proxy http://myproxy.com:80` if you don't have proxy.
```
chisel_windows_amd64.exe client -v --proxy http://myproxy.com:80 --keepalive 3s https://predix-chisel-postgres-dipteral-ksi.run.asv-pr.ice.predix.io 5000:10.131.54.5:5432
```
