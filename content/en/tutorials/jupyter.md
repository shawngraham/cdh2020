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
To actually open an .ipynb file properly, you need to have jupyter running on your machine. When I had you set up your [anlaytical toolkit](/tutorials/anaconda), I had you install Jupyter Notebooks from your terminal or anaconda command prompt with the command `$ pip install notebook`.

You can turn on a jupyter notebook _server_ (the environment for writing, running, and editing jupyter notebooks) at the command prompt with `$ jupyter notebook`.

When jupyter is running locally on your own computer, jupyter turns that text file into a webpage and shows it to you in your browser. Each block of code or text will be displayed; when you hit the 'run' button for a block of code that you've selected, jupyter will pass that code to the python (or whatever language) 'kernel' to do the processing, then display the results.

![](/images/jupyter-screen.png)

Jupyter notebooks can therefore _also_ be run on webservers, and accessed live on the web. So, we won't worry about running jupyter locally for the moment (reminders on how to do that are further below). Instead, we'll explore a series of notebooks written by the people behind JSTOR that aim to teach us _how_ to use jupyter notebooks, and the basics of writing/running python in a notebook. _Then_ I'll ask you to try turning [the Chronicling America script you wrote](/tutorials/apis/) into a notebook file.

The links below will launch a virtual computer in the cloud with Jupyter Notebooks already installed and ready to go. Do at least the **first** notebook, but push further if you're up to it, in order:

[Getting Started with Jupyter Notebooks](https://binder.tdm-pilot.org/v2/gh/ithaka/tdm-notebooks/master?filepath=getting-started-with-jupyter.ipynb)

The other notebooks will walk you through some of the basics of Python programming.

[Python Basics 1](https://binder.tdm-pilot.org/v2/gh/ithaka/tdm-notebooks/master?filepath=python-basics-1.ipynb)

[Python Basics 2](https://binder.tdm-pilot.org/v2/gh/ithaka/tdm-notebooks/master/?filepath=python-basics-2.ipynb)

[Python Basics 3](https://binder.tdm-pilot.org/v2/gh/ithaka/tdm-notebooks/master/?filepath=python-basics-3.ipynb)

Remember to hit 'file' and then 'save as', then 'file' and 'download as... ipynb' when you're done each one. **The notebook will time out if you leave it idle for too long**.

Move the ipynb file into your folder for your course work. Open a command prompt/terminal in that folder. Start jupyter with `$ jupyter notebook`. Do you see your ipynb file(s)? Click on the file name; the notebook will open in a new browser tab. You can now edit/add more code/delete etc. Change things up! When you hit 'save' though, your notebook is saving on your own computer, right?

You can put any ipynb file into your github repo. Github will generate a preview for it, which is cool.

Now: if you go back to the [the Chronicling America api discussion](/tutorials/apis/). See how I broke the writing of that script into several discrete steps? Each one of those steps could be a sequential code block in a Jupyter notebook. So why not try that, and add _markdown text_ blocks that describe what's going on; search for something of your own interest. Save your work, add it to your course repo.

...If you're feeling _really_ ambitious, [you can try any of these](https://docs.constellate.org/topic/research-notebooks/) as well.
