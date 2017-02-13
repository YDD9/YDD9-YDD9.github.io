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

conda can also manage environments, [see conda documents](https://conda.io/docs/using/envs.html).

[an example guide through](https://uoa-eresearch.github.io/eresearch-cookbook/recipe/2014/11/20/conda/)
### check current using env and all available envs

```
conda info --envs
```

### create an environment

```
conda create --name bunnies python=3 astroid babel
conda create --name snowflakes biopython

```

### clone an env

```
conda create --name flowers --clone snowflakes
```

### remove an env

```
conda remove --name flowers --all
```

### change env de/activate

```
Linux, OS X: source activate snowflakes
Linux, OS X: source deactivate

Windows: activate snowflakes
Windows: deactivate
```

### export env file and create env based on yml

```
conda env export > environment.yml
conda env create -f environment.yml
```
