---
title: "Digital History Analytical Toolkit"
description: "An environment for all kinds of data work"
date: 2020-01-28T00:10:51+09:00
draft: false
author: "Shawn Graham"
weight: 3
---

### Goal

- install python and conda tools on your machine
- install jupyter notebook 
- install R and R Studio

### Introduction

Cost = $0.

Anaconda is a platform for data science work. By installing Anaconda, you get access to a series of tools for working in both the Python and R programming languages, plus excellent visual interfaces that reduce (some of) the pain of programming.

When we do work in Python, we sometimes need to add more lego-pieces to what we're doing, to accomplish specific tasks. Anaconda comes with very nearly - but not all - of the pieces you might ever need. The thing is, different tasks might require the same pieces, and if you've ever fought with a sibling over who gets the good lego piece, well, that's what can happen inside your machine. Anaconda keeps everything playing nice by allowing us to create environments with copies of the pieces we need, separate from other copies, so that there are no conflicts.

Anaconda also gives us some excellent user interfaces for doing our work - juypter notebooks and R Studio - for instance. But... for our purposes, the full Anaconda installation, with all of its features, is more than what we need. In my experience, students' computers are littered with lots of software rarely used and rarely uninstalled; so if I ask you to install Anaconda with all of its features, we sometimes run into trouble. You _might_ want it, eventually, but for now, we're going to use a stripped down version called 'Miniconda' which will give us Python and some 'package managers'.

Python is the basic language, while a package manager lets us retrieve bundles of python code that have been developed for particular tasks, like working with tabular data efficiently. We'll also use a package manager to install Jupyter Notebooks.


### Download Miniconda

Go to [https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html) and download the **Python 3.8** installer for your computer. Run the installer and accept the defaults

### Anaconda Prompt and Terminal

When I ask you to use the 'command prompt' or the 'terminal', I am referring to the program that allows you to enter commands directly to your computer through typing.

+ **Windows users: I mean "anaconda prompt"**. You can find it in your list of programs/start menu. This is different than the program on your machine called 'command prompt'. The difference is that the Anaconda prompt knows where your python is installed, while the vanilla command prompt does not.

+ **Mac users** can use the terminal (which you can find under applications -> other -> terminal).

Going forward, when I want you to enter code at the **command prompt** or **terminal prompt**, I will signal this by starting the line with a `$`, like this:

`$ echo "this is what it looks like"`

**You do not type the $.**

Open a command prompt or terminal now and confirm that anaconda is installed:

`$ conda --version`

and that python is installed:

`$ python --version`

**if you get an error message, then anaconda did not install correctly _or_ Windows users you might not be in anaconda prompt.**

When we type 'conda' or 'python' or indeed 'echo' or anything else at the prompt, we are telling the computer that _this_ is a command. If we want the machine to run a particular python program we've written, we would invoke `python` followed by the program name, eg `example.py` so that the machine knows to open example.py with python.

### Install Jupyter Notebooks and R Studio


### Navigating the Commandline

When you click on a file in your windows explorer or in mac's finder, you click through nested folders, right? The path you take through those folders will look something like this: `username\example-folder\a-subfolder\file.txt`, although finder or explorer by default don't show you that.

When you open anaconda prompt or the terminal, how do you know where you are at? You print the working directory:

`$ pwd`

and that will give you the path to where you are working. To see what's inside the folder, you can:

`$ ls` list files, on a Mac or
`$ dir` directory, on a PC

Files will have an extension (eg, .txt, .doc) while directories won't. To go into a directory, we change directories:


`$ cd subfolder`


and we can go back up a level:

`$ cd ..`

![my terminal](/images/anaconda/my-terminal.png)

_My terminal, demonstrating a bit of moving around. Also some of the contents of my machine._

[More information on the terminal for mac](/tutorials/command-line-mac)

[More information on the command prompt for windows](/tutorials/command-line-win)
