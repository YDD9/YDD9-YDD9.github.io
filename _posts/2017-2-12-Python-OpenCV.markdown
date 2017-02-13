---
layout: post
title:  "Python openCV"
date:   2017-2-12 23:48:39 +0100
categories: Python
---


#Table of contents
[openCV installation](#installation)  


## openCV installation<a name="installation"></a>
[installation steps for mac](http://www.pyimagesearch.com/2015/06/15/install-opencv-3-0-and-python-2-7-on-osx/)  
Install OpenCV 3.0 and Python 2.7+ on OSX  
with Homebrew, virtualenv, virtualenvwrapper.  

Honestly, above installation is complicated and fail easily. I failed at cmake steps and decided to re-use conda to manage libs. At the beginning I didn't choose conda is because homebrew - conda gives warnings, although I didn't notice anything wrong. To overcome possible conflict, just temporarily disable the conda path in your file /.bash-profile when brew doesn't work.  

After I read this article [Conda: Myths and Misconceptions](https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/) I believe that I should not bother myself and I should use Anaconda distribution and Conda manages vitual env as well as all in/external libs.  
 
