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
- [SDK debug in a virtual environment](#SDK-debug-in-a-virtual-environment)


# Conda Config<a name="Conda-Config"></a>
The [conda configuration](https://conda.io/docs/user-guide/configuration/sample-condarc.html) file, _.condarc_, is an **optional** runtime configuration file that allows advanced users to configure various aspects of conda, such as which channels it searches for packages, proxy settings and environment directories.

[proxy settings](https://conda.io/docs/user-guide/configuration/use-winxp-with-proxy.html):
```
proxy_servers:
  http: http://user:pass@corp.com:8080
  https: https://user:pass@corp.com:8080

ssl_verify: False
```


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

# Create a virtual environment for development<a name="Create-a-virtual-environment-for-development"></a>
The most common ways are to either use 
```
conda create --name <env_name> <packages to install optional>
# https://medium.freecodecamp.org/why-you-need-python-environments-and-how-to-manage-them-with-conda-85f155f4353c
conda create -n my_virtual_env python==2.7
# or 
conda env create -f environment.yml
```   

To clone an env 
```
conda create --name flowers --clone snowflakes
```  

To create an env from an env saved in [Anaconda repository](https://docs.anaconda.com/anaconda-cloud/user-guide/tasks/work-with-environments) 
```
conda env create --name flowers snowflakes
```

You must have an Anaconda account and have `anaconda -h` installed, `anaconda login` then upload your env.yml then later you can load it from anywhere.   
To export yml of myenv to current path:
```
conda env export -n myenv -f myenv.yml
# or
activate myenv
conda env export > environment.yml
```

To install requirements.txt
```
conda install --yes --file requirements.txt
# any package fails, will quit. Work around:

while read requirement; do conda install --yes $requirement; done < requirements.txt
```


To remove an environment:  
```
conda remove --name myenv --all
```

To see a list of all packages installed in a specific environment:
```
# to know your env names
conda info --env
```

If the environment is not activated:
```
conda list -n myenv
```

If the environment is activated:
```
conda list
```

To see if a specific package is installed in an environment:
```
conda list -n myenv scipy
```


To create with specific python version or multiple packages, [link](https://conda.io/docs/user-guide/tasks/manage-environments.html)    

Then setup development folder and initialize with git and now activate the virtual env    
Linux, OS X: source activate snowflakes   
Linux, OS X: source deactivate   

Windows: activate snowflakes    
Windows: deactivate   


To pip install package into conda virtual env
As pip install packages into where pip exists, when you create the conda virtual env, you should install the pip at creation<br />
If pip is installed after conda virtual env is created, it won't work.<br />
```
conda create -n sciEnv numpy pip 
conda activate sciEnv
```


# SDK debug in a virtual environment<a name="SDK-debug-in-a-virtual-environment"></a>
[In case of VScode](https://medium.com/@kylehayes/using-a-python-virtualenv-environment-with-vscode-b5f057f44c6a)<br/>
You can verify the setting by hitting `Command/Ctrl +`, to open settings. The right panel will show you two tabs: **User Settings**, and **Workspace Settings**. <br/>
Click Workspace Settings. This will show you a simple JSON document with one of the attributes labeled as “python.pythonPath”. This is path should say something along the lines as:
```
${workspaceFolder}/env/bin/python
```
[In case of Pycharm](https://ohadp.com/pycharm-debug-inside-a-pip-in-a-virtual-env-97aa98eac77f)
