---
layout:     post                    # 使用的布局（不需要改）
title:      Python-Virtualenv           # 标题 
subtitle:   Joker #副标题
date:       2019-12-28              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [python,django]
---

# **Create Python-Virtualenv in Windows10**
This is the PIP acceleration `-i https://pypi.tuna.tsinghua.edu.cn/simple`

## *Why build a virtual environment?*

Because: `If you want to develop several different projects on one computer, `
`you need to use different versions of the same package. `
`If you use the above command and install or update in the same directory, `
`the new version will overwrite the previous version, and the other projects will not run.` 

# Create Virtualenv in Linux

```s
# Open a terminal
sudo pip3 install virtualenv
sudo pip3 install virtualenvwrapper

```

# Create Virtualenv in Windows10
```s
# Open a terminal
pip3 install virtualenv
pip3 install virtualenvwrapper-win
```
> This is installing the virtual environment


# **In Linux**
`After installing the virtual environment, `
`if the mkvirtualenv command cannot be found,`
`the environment variable must be configured:`
```s
# 1. Create a directory to hold the virtual environment
mkdir $HOME/.virtualenvs

# 2. Open ~/.bashrc file and add the following:
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh

# 3. run
source ~/.bashrc
```
```s
# Creating a virtual environment python2
mkvirtualenv 'virtual name'
case:
mkvirtualenv py3_django

# Creating a virtual environment python3
mkvirtualenv -p python3 py3_django

# workon To view
case:
workon # View the virtual environment
# use workon
workon ...
case:
workon py3_django
```
# **In Windows10**

`Use git bash if you know  how to use git bash`
`We use it here git bash`

```s
# 1. Creating a virtual environment
virtualenv -p python py3_django

# 2. Enter the virtual environment
# source 'An absolute path'...
source py3_django/Scripts/activate

```
[CMD command](https://www.cnblogs.com/accumulater/p/7110811.html)
[Git Install](https://git-scm.com/)
> Creating a virtual environment

# **Use workon in CMD**

`Add system environment variables`

```s
# Add system environment variables
variable: WORKON_HOME
value: D:\python_virtual # Enter workon later and you'll see the virtual environment in that 

# use workon ...
workon py3_django

```

~~Hey Miss Liu This Rose Is For You~~