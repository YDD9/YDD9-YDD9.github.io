<!DOCTYPE html>
<html>
<body>

<h1>Installation of jekyll on MAC</h1>
<p>brew install ruby<br>
ruby -v</p>

<p>gem install jekyll<br>
jekyll won't work</p>

<p>This is a side effect of Apple's new rootless (aka System Integrity Protection or SIP) feature in OS X El Capitan, 
but it does not affect /usr/local/bin.<br>
<a href="http://stackoverflow.com/questions/31567029/how-can-i-install-jekyll-on-osx-10-11">stackoverflow link</a></p>

<p>sudo gem install -n /usr/local/bin/ jekyll<br>
jekyll -v</p>

<p>This tells gem to install Jekyll into a folder that isn't protected by SIP, rather than the default protected location 
under /Library/Ruby/Gems.</p>


</body>
</html>