---
title: "Jupyter and Python"
description: "A platform, a language"
date: 2020-01-28T00:10:51+09:00
draft: false
author: "Shawn Graham"
---

## Introduction

A jupyter notebook is a way of working with code (in a variety of programming languages) in discrete cells, alongside text, images, figures, videos, and links (written using markdown). Sometimes this is called 'literate programming' because it allows you to weave your narrative about the work, the transformations you're making, the analysis you're doing, around the code itself. In this way, a reader progressing from start to bottom can both replicate your work and reproduce it in other contexts.

Under the hood, a jupyter notebook file - which has the file extension `.ipynb` - is a text file that uses pairs of keys and values for those keys to describe the text, the code, and anything else in the notebook. You can open .ipynb files with your text editor. They'll look something like this:

```
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Introduction\n",
    "\n",
    "In this notebook, I am experimenting with tf-idf on diaries of June Malloy of Pictou.\n",
    "\n"
   ]
  },
```
To actually open an .ipynb file properly, you need to have jupyter running on your machine.

When jupyter is running locally on your own computer, jupyter turns that text file into a webpage and shows it to you in your browser. Each block of code or text will be displayed; when you hit the 'run' button for a block of code that you've selected, jupyter will pass that code to the python (or whatever language) 'kernel' to do the processing, then display the results.

![](/images/jupyter-screen.png)

Jupyter notebooks can therefore _also_ be run on webservers, and accessed live on the web. So, we won't worry about running jupyter locally for the moment (instructions on how to do that are further below). Instead, we'll explore a series of notebooks written by the people behind JSTOR that aim to teach us _how_ to use jupyter notebooks, and the basics of writing/running python in a notebook.

The links below will launch a virtual computer in the cloud with Jupyter Notebooks already installed and ready to go. Do at least the **first** notebook, but push further if you're up to it, in order:

[Getting Started with Jupyter Notebooks](https://binder.tdm-pilot.org/v2/gh/ithaka/tdm-notebooks/master?filepath=getting-started-with-jupyter.ipynb)

[Python Basics 1](https://binder.tdm-pilot.org/v2/gh/ithaka/tdm-notebooks/master?filepath=python-basics-1.ipynb)

[Python Basics 2](https://binder.tdm-pilot.org/v2/gh/ithaka/tdm-notebooks/master/?filepath=python-basics-2.ipynb)

[Python Basics 3](https://binder.tdm-pilot.org/v2/gh/ithaka/tdm-notebooks/master/?filepath=python-basics-3.ipynb)

Remember to hit 'file' and then 'save as', then 'file' and 'download as... ipynb' when you're done. **The notebook will time out if you leave it idle for too long**. You can put the ipynb file into your github repo. Github will generate a preview for it, which is cool.

...If you're feeling ambitious, [you can try any of these](https://docs.constellate.org/topic/research-notebooks/) as well.

### Install with Anaconda
To run or create .ipynb notebooks locally, you need to have jupyter installed locally.

Since you've already installed Anaconda, start up Anaconda Navigator, and hit the 'install' button under the 'jupyter notebook' tile. Once it has installed, you can launch it by hitting the button again. Alternatively, you can start up an instance of jupyter by typing `$ jupyter notebook` at the command prompt.

![](https://docs.anaconda.com/_images/nav-defaults.png)

At this point, if you come across a file with .ipynb as its extension, you can open it and run it in a Jupyter notebook or lab. Try it now - [right-click on this link and save-as](https://github.com/shawngraham/dhmuse-notebooks/raw/master/python-basics-1.ipynb) to your computer. Then, open a command line in the folder where you saved the file. Start up `$ jupyter notebook` - which opens a new tab in your browser - and then click on the file you downloaded. Now you can run and save your work locally!

#### An alternative way to install Jupyter Notebook 

If for whatever reason you don't have Anaconda or don't want to bother with having the whole Anaconda installation on your machine, you can use [Miniconda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/#regular-installation) instead - see this video by Chantal Brousseau:

<div align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/10FPoTCcv4I" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></div>
