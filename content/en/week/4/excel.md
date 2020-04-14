---
title: "Excel & R"
description: "Of Macroscopes and Microscopes"
date: 2020-01-28T00:10:37+09:00
draft: false
weight: -5
---



Get some quantitative stuff, a census or something. Show some basics (nb not teaching stats)

basic counting in R. Basic plots

### Some Basic Counting and Plotting in R  

Start up your RStudio, and make a new R script. The first thing we're going to do is get set up so that we can import some data directly from the web. We use the `RCurl` package to do that:

```R
install.packages("RCurl")
library("RCurl")
```
(Remember to hit the `run` button for each line.)

We're going to reach out and grab a table of data (the colonial newspapers database you encountered earlier) and import it into R. We do that like this:

```R
x <- getURL("https://raw.githubusercontent.com/shawngraham/exercise/gh-pages/CND.csv", .opts = list(ssl.verifypeer = FALSE))
```
Notice you now have a variable in your 'Global Environment' pane. We need to extract the data from that, which we do with this:

```R
documents <- read.csv(text = x, col.names=c("Article_ID", "Newspaper Title", "Newspaper City", "Newspaper Province", "Newspaper Country", "Year", "Month", "Day", "Article Type", "Text", "Keywords"), colClasses=rep("character", 3), sep=",", quote="")
```

We read the csv file from x, and create the columns into which the data is poured; all of this is now in `documents`. When we only want information from a particular column, we modify the variable slightly (eg. `head(documents$Keywords)` would return the first few rows of the information in the keywords column).

Now we can do some things. Let's count the number of documents by the city in which they were published:

```R
counts <- table(documents$Newspaper.City)
counts
```
The first `counts` creates the variable; the second `counts` on its own reveals what's inside the variable.

Now let's plot that:

```R
barplot(counts, main="Cities", xlab="Number of Articles")
```

The plot will appear in the bottom right pane of RStudio. You can click on 'zoom' to see the plot in a popup window. You can also export it as a PNG or PDF file. Clearly, we’re getting an Edinburgh/Glasgow perspective on things. And somewhere in our data, there’s a mispelled ‘Edinbugh’. Do you see any other error(s) in the plot? How would you correct it(them)?

Let's do the same thing for year, and count the number of articles per year in this corpus:

```R
years <- table(documents$Year)
barplot(years, main="Publication Year", xlab="Year", ylab="Number of Articles")
```

There’s a lot of material in 1789, another peak around 1819, againg in the late 1830s. We can ask ourselves now: is this an artefact of the data, or of our collection methods? This would be a question a reviewer would want answers to. Let’s assume for now that these two plots are ‘true’ — that, for whatever reasons, only Edinburgh and Glasgow were concerned with these colonial reports, and that they were particulary interested during those three periods. This is already an interesting question that we as historians would want to explore.

Try making some more visualizations like this of other aspects of the data. What other patterns do you see that are worth investigating?

This [page](https://rstudio-pubs-static.s3.amazonaws.com/7953_4e3efd5b9415444ca065b1167862c349.html) shows you the code for some other basic visualizations. See if you can make some more visualizations. I've created a [tarsus.txt](/data/tarsus.txt) file and a [unicorn.txt](/data/unicorn.txt) file so that you can see how his code works (although you might wish to open both files in your sublime text editor and add more rows of data; careful: columns are separated by tabs.)
