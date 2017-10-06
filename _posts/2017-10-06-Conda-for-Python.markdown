---
layout: post
title:  "Conda for Python"
date:   2017-10-06 14:48:39 +0100
comments: true  
categories: Conda Python
---



# Table of contents

- [Conda Config](#Conda-Config)  
- [Create a virtual environment for development](#Create-a-virtual-environment-for-development)



# Conda Config
The [conda configuration](https://conda.io/docs/user-guide/configuration/sample-condarc.html) file, _.condarc_, is an **optional** runtime configuration file that allows advanced users to configure various aspects of conda, such as which channels it searches for packages, proxy settings and environment directories.

To check the where is your _.condarc_ file, `config info` and look for config file. If it's None, you can then create one with YAML syntax under your home path `echo %USERPROFILE%` or `echo $HOME` . A sample _.condarc_ file you can find [link](https://conda.io/docs/user-guide/configuration/sample-condarc.html)

Config priority where to look for packages:  
By default, conda now prefers packages from a **higher priority channel** over **any version** from a **lower priority channel**. Therefore, you can now safely put channels at the bottom of your channel list to provide additional packages that are not in the default channels, and still be confident that these channels will not override the core package set.

The following command adds the channel “new_channel” to the top of the channel list, making it the highest priority:
```
conda config --add channels new_channel
```

Conda now has an equivalent command:
```
conda config --prepend channels new_channel
```

Conda also now has a command that adds the new channel to the bottom of the channel list, making it the lowest priority:
```
conda config --append channels new_channel
```

To make conda only consider version number and ignore channell priority:
```
conda config --set channel_priority false
```

# Create a virtual environment for development 
The most common ways are to either use `conda create --name <env_name> <packages to install optional>` syntax or create from a file using `conda env create -f environment.yml` syntax.
To clone an env `conda create --name flowers --clone snowflakes` or `conda env create --name flowers snowflakes`  
To create with specific python version or multiple packages, [link](https://conda.io/docs/user-guide/tasks/manage-environments.html)
