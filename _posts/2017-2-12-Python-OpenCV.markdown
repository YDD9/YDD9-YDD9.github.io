---
layout: post
title:  "Python openCV"
date:   2017-2-12 23:48:39 +0100
comments: true  
categories: Python openCV image
---


#Table of contents
[openCV installation](#installation)  


## openCV installation<a name="installation"></a>
[installation steps for mac](http://www.pyimagesearch.com/2015/06/15/install-opencv-3-0-and-python-2-7-on-osx/)  
Install OpenCV 3.0 and Python 2.7+ on OSX  
with Homebrew, virtualenv, virtualenvwrapper.  

Honestly, I can't believe the steps are so complicated. I failed at cmake steps and decided to use conda to manage packages. Since pip is by design only for python, choose it to manage other packages is just not using the right tool to do the work. 

If you're using both conda and brew on your mac, just remember to disable conda in your $PATH when using brew:

```
cd ~
open .bash_profile
```

Your .bash_profile is open with below info inside, just comment the anaconda2 path line.  
```
# homebrew
export PATH="/usr/local/bin:$PATH"

# added by Anaconda2 4.3.0 installer
export PATH="/Users/Chris/anaconda2/bin:$PATH"
```


<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/

var disqus_config = function () {
this.page.url = {{page.url}};  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = {{page.id}}; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://https-ydd9-github-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>

<script id="dsq-count-scr" src="//https-ydd9-github-io.disqus.com/count.js" async></script>