---
layout: post
title:  "3 Quick Steps to Installing multiple Python versions"
date:   2018-12-25 10:04:04
author: Ayush Vatsyayan
categories: Python
tags:	    python
cover:  "/assets/python_env.png"
---
# Why Do You Need Virtal Environment?
Imagine you have an **application that needs version 2.7 of Python, but another application requires version 3.0**. How can you use both these applications? 

Whhat if you want to **install an application and leave it be?** If an application works, any change in its libraries or the versions of those libraries can break the application.

Creating multiple python virtual environments solves this issue by creating an environment that **has its own installation directories** and that **doesn’t share libraries with other installed python environments**.

In summary, Virtualenvs makes it easier to work on more than one project at a time without introducing conflicts in their dependencies.

---

# What is Virtual Environment?
Virtual Environment is a cooperatively isolated runtime environment that allows Python users and applications to install and upgrade Python distribution packages without interfering with the behaviour of other Python applications running on the same system. This means that you can have two versions of python installed on the same machine.

The `venv` module provides support for creating lightweight “virtual environments” with their own site directories, optionally isolated from system site directories. Each virtual environment has its own Python binary (which matches the version of the binary that was used to create this environment) and can have its own independent set of installed Python packages in its site directories.

---

# Steps to create Virtual Environment
**<u>Step 0</u>:** Check `python` is installed:
```shell
python3 --version
```

**<u>Step 1</u>:** Decide upon a directory where you want to place it, and run the `venv` module as a script with the directory path:
```shell
python3 -m venv sample-env
```
**<u>Step 2</u>:** Activate virtual environment:
```shell
source sample-env/bin/activate
```
Activating the virtual environment will change the shell’s prompt to show what virtual environment you’re using For example: 
> `(sample-env) myuser-MacBook-Pro:~ root$`

At this step python will now be accesible via `python` command. You can install and remove the packages using `pip`, which will also become available in the new environment.

**<u>Step 3</u>:** If you are done working, you can deactivate the virtual environment:
```
deactivate
```
At this point your prompt will change back to normal.

# References
* <https://www.python.org/dev/peps/pep-0405/>
* <http://www.virtualenv.org>
* <https://xkcd.com/1987/>
