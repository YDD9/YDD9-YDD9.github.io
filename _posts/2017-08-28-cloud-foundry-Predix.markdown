## cf-uaac installation

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



## cf-uaac troubleshoot

When executing command `uaac target https://*.predix-uaa.run.asv-pr.ice.predix.io`, error "Use '--skip-ssl-validation' to continue with an insecure target displays" occurs, the work around is set certificate before running scripts 
`set CERT_FILE_PATH="C:\Users\YDD9\Downloads\cmder\vendor\git-for-windows\usr\ssl\certs\ca-bundle.crt"`   
or `export SSL_CERT_FILE="/Users/YDD9/Downloads/cmder/vendor/git-for-windows/usr/ssl/certs/ca-bundle.crt"` for mac users   
Further reading materials [Predix forum solution](https://forum.predix.io/questions/11900/solution-uaac-target-failed-to-access-invalid-ssl.html) and [Predix advice for java](https://forum.predix.io/articles/11434/ssl-certificate-problem-1.html)  

The best workaround I found is proposed by [cloudfoundry doc](https://discuss.pivotal.io/hc/en-us/articles/115009446348-Deploying-Spring-Cloud-Services-Broker-fails-with-UAAC-target-error) is `uaac target --ca-cert "C:\Users\YDD9\Downloads\cmder\vendor\git-for-windows\usr\ssl\certs\ca-bundle.crt" https://*.predix-uaa.run.asv-pr.ice.predix.io`  
The idea is to find the path of your ca-bundle.crt file(normally in your gitbash path or similar bash path) and force UAAC command to use it.  
  
