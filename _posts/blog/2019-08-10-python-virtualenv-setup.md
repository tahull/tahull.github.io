---
layout: post
categories: [setup guide]
tags: [virtualenv, linux, python, ubuntu 18.04]
permalink: /blog/:year/:month/:title
featured-img: /images/features/shell-feature.png
---

Python by itself is awesome and packages add a lot of functionality. Installing all those packages makes a mess and some of them don't play well with others, or maybe you need a certain version of a package for a project. Isolating is the way to go, virtualenv is one way to manage that.

## Tangent
One of the things I don't like about using a Ubuntu LTS, rather than a distribution with rolling releases, is that many apt packages are locked in to the last stable version of the app. One day I thought I would do some work in python, I used pip to install a package, and pip dropped a message that I should update pip. So I checked for updates in `apt-get`, no new pip version there, so I followed the pip instruction to update through `pip install --upgrade pip`... then it broke. So much for being productive, lesson is don't upgrade `apt` managed packages outside the `apt` package manager. The issue after doing an update that way is:
```shell
from pip import main
ImportError: cannot import name 'main'
```
Fixing this is easy once you find someone else's solution, either remove and reinstall python3-pip or use pip as a python package rather than standalone command.
[https://stackoverflow.com/questions/49836676/error-after-upgrading-pip-cannot-import-name-main](https://stackoverflow.com/questions/49836676/error-after-upgrading-pip-cannot-import-name-main)

Standalone pip
```shell
pip3 list
```
Vs. running pip through python
```shell
python3 -m pip list
```


## Install virtualenv
Install this one global pip package, and no other's if you can help it.

```shell
python3 -m pip install virtualenv
```
It should be runnable from command line after that

```shell
virtualenv --version
```
## Create a virtualenv
Choose where to setup your next project location and change directory to that location. It seems standard to name the virtualenv folder env or ENV, but it can be named something else.
```shell
virtualenv ~/Documents/python/newfolder/env
```

## Activate the virtualenv
Each time you want to use a virtual environment, you will have to activate it for that session.
```shell
source ~/Documents/python/newfolder/env/bin/activate
```

We can confirm the paths are set for this virtualenv.
```shell
$ which pip
.../Documents/python/newfolder/env/bin/pip
$ which python
.../Documents/python/newfolder/env/bin/python
```
# Install packages
Installing packages will put the packages into the activated virtualenv, no mess, or version problems. As an example:
```shell
pip install scipy numpy pandas
```

## Deactivate the virtualenv
When done with the virtualenv close the shell or deactivate the virtualenv.
```shell
deactivate
```

## More
Check the docs  [https://pypi.org/project/virtualenv/]( https://pypi.org/project/virtualenv/)
There's options to set a specific python to use, like if you want set the virtualenv to use python2
or python3, or make the virtualenv project folder relocatable.
