---
layout: post
title:  "Nodejs from zero!"
date:   2020-07-25 21:49:39 +0100
comments: true  
categories: web
---


# Table of contents
1. [Install in Ubuntu](#installinubuntu)





## Install in Ubuntu <a name="installinubuntu"></a>
For Ubuntu16.04.x, use snap to install and update: https://github.com/nodesource/distributions/blob/master/README.md#snapinstall
```
$ sudo snap install node --classic --channel=8    
$ sudo snap refresh node --channel=10  
```   

Substituting `8` for the major version you want to install. Both LTS and Current versions of Node.js are available via snapcraft.

The `--classic` argument is required here as Node.js needs full access to your system in order to be useful, therefore it needs snap’s "classic confinement". 

You can use the `refresh` command to switch to a new channel at any time. Once switched, snapd will update Node.js for the new channel you have selected.

