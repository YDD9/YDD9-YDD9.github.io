install cf-uaac on win10 is not forward straight, especially when there's certain version of Ruby that doesn't work well.

Basically you have to follow this wiki https://github.com/cloudfoundry/cf-uaac/wiki to install Ruby2.3.x, Ruby-devkit-mingw64/86-x and cf-uaac.

Welcome to the cf-uaac wiki!

Installation:

There has not been much information about what needs to be installed before you run "gem install cf-uaac", so here are some pre-reqs.

Windows:

Download and install RubyGems (http://rubyinstaller.org/) (Download the installer AND the devkit)
Installer: Run the installer. Make sure you tick all of the boxes regarding the PATH and stuff.
DevKit: I would create a folder at C:/ruby-devkit. Extract the self-extracting file to that location.
Open CMD (or whatever) and cd to your devkit location. Install it (“ruby dk.rb init” and “ruby dk.rb install”)
Install cf-uaac (“gem install cf-uaac”)




Version 2.4.x Ruby doesn't work when you do `ruby install cf-uaac` could be similar issues as https://github.com/rubygems/rubygems/issues/977

Predix also use cf-uaac.
