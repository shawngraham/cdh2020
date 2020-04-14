---
title: "Topic Models"
description: "Of Macroscopes and Microscopes"
date: 2020-01-28T00:10:37+09:00
draft: false
weight: -2
---
### An Introduction to Topic Models

{{<youtube gN2x_KjJI1o >}}

_that link at the end, if you're interested: [http://bit.ly/sw-tm-tour](http://bit.ly/sw-tm-tour)_

### Topic Modeling with the TMTool

There are many tools that can fit a topic model to your text. The easiest one to use is a graphical user interface overlaid on the [MALLET Toolkit](http://mallet.cs.umass.edu/) called ['The Topic Modeling Tool'](https://senderle.github.io/topic-modeling-tool/documentation/2017/01/06/quickstart.html) by Jonathan Enderle.

1. Follow Enderle's instructions for downloading and installing the tool [here](https://senderle.github.io/topic-modeling-tool/documentation/2017/01/06/quickstart.html)
2. Make a new directory on your machine called `tmt`. Make a subdirectory in `tmt` called `input`.
3. Make a new subdirectory in `tmt` and call it `output`.
4. Download, unzip, and then copy the text files from the [Scottish Chapbooks dataset](/data/nls-text-chapbooks.zip) into your `input` directory.
5. Start up the Topic Modeling Tool; you'll see this interface:
![interface](https://senderle.github.io/topic-modeling-tool/images/main-tool-window.png)
6. Click on ![](https://senderle.github.io/topic-modeling-tool/images/input-dir-button.png) and select your `input` folder **but only click once.** If you double click, you go _into_ the folder, and that's not what you want.
7. Select where you want the output from the process to go. Click on the output button, and select your `output` directory.
8. Decide on how many topics you want to look for, and then click 'learn topics'
![learn topic](https://senderle.github.io/topic-modeling-tool/images/number-of-topics.png)

And soon the console will load up with results. If you watch carefully, you'll see the console build up the original command that it feeds to MALLET (and which, if you were working with MALLET at the command line, you'd have to put together yourself). It will run through several iterations (which you can tweak on the 'optional settings' button) to try to find the best fit of your number of topics to the data.

### But what does it mean?

Ah, well... for that, please read ['Very Basic Strategies for Interpreting Results from the Topic Modeling Tool'](http://miriamposner.com/blog/very-basic-strategies-for-interpreting-results-from-the-topic-modeling-tool/) by Andy Wallace; follow that up Enderle's section on [analyzing the output](https://senderle.github.io/topic-modeling-tool/documentation/2017/01/06/quickstart.html) from his quickstart guide (scroll down).

Click on your `output` folder, and then the `output_html` folder you'll find inside. Click on the `index.html`; this will open in your browser, and it will show you the topics it found as lists of each topic's keywords; click on a topic, and it'll show you the relevant documents; click on a document and it'll give you a text snippet for that document, plus clickable links to the _other_ documents present in that document. It's a bit of a topic browser, and lets you cycle from distant to close and back again as you explore your results.  

A bit of visualization is always useful though. Try making a pivot table, as Enderle describes. Make some charts, as Wallace describes. What decisions do you have to make, to make sense of the materials? What _other_ data might you need in order to figure out what these patterns might mean?

### Topic Model in R

The Topic Model Tool is an excellent tool for building and exploring topic models. But you might want to try building a topic model in R. R is a powerful language for statistical exploring, visualizing, and manipulating all kinds of data, including textual. While R can be worked with from the command line, RStudio is what we need to preserve our sanity. RStudio can be started up from your Anaconda Navigator - you met RStudio back in week 2 remember.

R allows us to do scripted analyses. We write out the sequence of transformations or analyses to be done, making a kind of program that we can then feed our data into. Once we have a workflow that does what we need it to do, it becomes trivial to re-use the code on other datasets. What's more, when we do an analysis, we can publish the scripts and the data. We don't have to taken an author's word for it: we can re-run the analysis ourselves or build upon it.

This course only touches in the lightest manner on the potential for R for doing digital history. Please see [Lincoln Mullen's Computational Historical Thinking with Applications in R](http://dh-r.lincolnmullen.com/) for more instruction if you're interested.
