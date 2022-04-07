---
title: "Cleaning & Manipulating Data with R"
description: "Working with Messy Data"
date: 2020-01-28T00:10:37+09:00
draft: false
weight: 4
---

## Cleaning and Manipulating Data with R

We're going to use data from pulled from the [The Survey of Scottish Witchcraft Database](http://www.shca.ed.ac.uk/Research/witches/) for this tutorial; this tutorial was written by Chantal Brousseau.

So maybe you've created a dataset by scraping the web, or found some really cool open data but... it's messy! You try to make visuals with it, but there's so many nonsense characters and repeating words that your data seems meaningless!

Well, no need to fear, R makes cleaning all that nonsense up simple! At first glance, if you notice your data has a lot of names or words that are recognizable misspellings or jumbles of a singular word, it may be beneficial to [use OpenRefine first to reconcile these occurences](https://handsondataviz.org/open-refine.html).

Now, to start with R, first we'll want to install a few packages:


```R
install.packages("tidyverse")
install.packages("tidytext")
```

A **package** is a bundle of code and data made to perform a specified task which you can load or "import" into your own project to use.

For this task, we're using the [Tidyverse](https://www.tidyverse.org/) collection, which is a collection of R packages designed to be used for data science. If you're wondering "what IS 'clean' data?", they also have a [handy guide](https://tidyr.tidyverse.org/articles/tidy-data.html) on defining that too.

We will later use the package [tidytext](https://github.com/juliasilge/tidytext) which applies the tidy data principales to [text mining](https://www.tidytextmining.com/preface.html#preface).

In your own projects, after installing Tidyverse, you can import individual packages from within the collection as needed, OR you can simply import the entire collection as I'm about to do:


```R
library(tidyverse)

# this is a part of Tidyverse too-- I'll touch on it shortly
library(magrittr)

library("tidytext")

```

If you know you only need to use one or two specific packages, it's best practice to only import the ones you need. For each task, I'll indicate what specific package is being used (set off with a # to indicate to R not to run it at the moment) so you know what packages you can apply to your own work!

## Loading your data

Now that we have the tools we need, we can start cleaning our data by "reading" it in to the program. We're going to use [this data](/data/fairy-elements.csv); right-click and save as to the folder where you're working with R OR copy the url and change the code below appropriately. (Later on, you'll want ['devilappearence.csv'](/data/devilappearence.csv) too.)


```R
# library(readr)  # remember you can always install.package("readr")
                # or whatever you need if you get a package not found error

fairyElements <- read_csv("fairy-elements.csv")

# if your data is from a link, replace 'read_csv' with 'read_url'
```

The part before the arrow, `fairyElements`, is a **variable**. In programming, variables act like buckets, holding whatever you give them through assignment operators such as `<-` "inside". In this case, we are storing the contents of `fairy-elements.csv`, which is data pulled from the [The Survey of Scottish Witchcraft Database](http://www.shca.ed.ac.uk/Research/witches/) on elements associated with fairies found in trial records from the Scottish witch-hunts occuring during the period of 1563 to 1736.

We put this in a variable arranged in a special kind of data table (a tibble) so we can easily manipulate it when cleaning its contents. To see your data, you can type:


```R
# library(tibble)
fairyElements <- as_tibble(fairyElements)
# Note: You can overwrite variables like I just (intentionally) did for fairyElements-- pay attention when naming variables so you don't do this unintentionally!

# Check out your data! This will show the first couple of rows
head(fairyElements)
```

So, when looking over this table's contents, what do you notice?

Immediately, I'm seeing that the location `Nidrie` always begins with a `?`, and there are also some `NA` values, which indicates a lack of data (can also appear as `NULL`), in the `Case Notes` column. Unusually placed characters and `NA` values are both common occurences in the data we'll be working with so we'll be looking at how to remove or modify them, but first, look at the column names-- they have spaces between each word!

## Modifying Column Names

Spaces between words are good for readability, but can make working with the data more difficult. Run this and see what happens:


```R
# the $ indicates that you're pulling something that is stored within the variable that comes before it-- in this case, you're trying to pull a column from our 'fairyElements' table

fairyElements$Motif Type
```

It throws an `unexpected symbol` error because it interprets `Motif Type` as 2 separate words due to the space, and so the program is confused because it's seeing this "random" word sitting on the line you're running and doesn't know what to do with it.

Thus, before we tackle cleaning any of the actual data, let's clean up the column names:


```R
# library(magrittr) for the pipe (aka '%<>%')
# library(stringr) for str_replace_all() function

colnames(fairyElements) %<>% str_replace_all("\\s", "_") %<>% tolower()

# now look at your columns!
head(fairyElements)
```

`colnames()` is saying that you want the column names from `fairyElements`
`%<>%` is a **pipe** in R, and is like saying "update"-- this whole line is essentially read as:
    - Update (`%<>%`) the column names in `fairyElements` after replacing all spaces (`\\s`) with underscores (`_`) and update again when the column names are made lowercase (`tolower()`)

Functions from the `stringr` package use regular expressions (the same mechanism behind "find and replace" in text editors like Sublime Text and Atom) to search for _patterns_ that you specify. Regular expressions can be tricky, but, luckily, Tidyverse created [a convinient cheatsheet on using `stringr`](https://evoldyn.gitlab.io/evomics-2018/ref-sheets/R_strings.pdf) that includes a guide to regular expressions on the 2nd page if you need to find something other than spaces (`\\s`).

## Removing Missing Values (`NA` and `NULL`)

With that in mind, we can now move on to actually cleaning the data itself, which will also make use of `stringr`. First, let's look at thos `NA` values. There are two ways you can clean this up:

```R
# library(tidyr)

# Replace with a word
# Note: if you have a "NULL" value, you can typically just use the str_replace_all() function

fairyUnknown <- fairyElements %>% replace_na(list(case_notes = "Unknown"))

print("NA is replaced with 'Unknown':")
head(fairyUnknown)

# Remove row entirely
fairyRemove <- fairyElements %>% drop_na(case_notes)

print("Rows with NA removed entirely: ")
head(fairyRemove)
```

Choosing which method to use is entirely up to you. You may want to consider the number of "Unknown" factors when analyzing your data, in which case you should probably simply replace the missing valuess with a more meaningful word or statement. If you feel any missing data at all will hurt your analysis, you can go ahead and just remove the values, but remember to consider this when looking at your final results.

I'm going to replace the `NA` values with "Unspecified" because I feel that the number of witch trials that have no notes attached to them could potentially be meaningful.

## Modifying Column Content

Next, we can look at fixing incidents of `?Nidrie`:


```R
fairyElements <- fairyElements %>% replace_na(list(case_notes = "Unknown"))

fairyElements$case_notes %<>% str_remove_all("\\?")

# note: a pipe pushes the object preceding it (in this case 'fairyElements$case_notes') as the first argument of the function that follows it

head(fairyElements)
```

And with that, our data is clean enough to begin further analysis! I'll now show you a couple of basic ways you can manipulate your data and find new perspectives.

## Counting

The `count()` function forms the basis of many types of analysis, as it allows you to count the unique values of one or more variables. For example, let's say I want to see the most common motif featured in the witch trials contained in `fairyElements`:


```R
motif <- fairyElements %>%
    count(motif_type, sort = TRUE)

# 'sort = TRUE' means that the table created from this will be sorted from largest to smallest count

head(motif)
```

Pretty simple right? You can do this for columns that contain larger bodies of text as well with just a few extra steps to produce meaningful results. Take `folk_notes` for instances:


```R
# what happens if we do the same thing as before?
folkNotes <- fairyElements %>%
    count(folk_notes, sort = TRUE)

head(folkNotes)
```

It certainly showed us that there are many individuals from the same trial in this dataset, but I wanted word frequencies, not entire-paragraph frequencies! To get that we need to first break the text into individual **tokens**, which are meaningful units of text, so, usually a word. Tidytext gives us an easy function to do this:


```R
# Create an ID column
fairyElements$id <- 1:nrow(fairyElements)

# make a data frame with just the IDs and contents of 'folk_notes'-- I noticed there are more odd "?" in this column so I included this cleaning step within this action
folkNotes <- tibble(id = fairyElements$id, text = (str_remove_all(fairyElements$folk_notes, "\\?")))

# now we'll use tidytext's unnest_tokens() function to tokenize the text
folkNotes <- folkNotes %>%
  unnest_tokens(word, text)

# let's look at the first couple tokens
head(folkNotes)
```

Looks good! Now, tokens are meant to be *meaningful*, and as you can see, there are words such as 'a' or 'by' which will likely not be useful for our analysis. To get rid of these types of tokens, which are known as "stop words", we can load the `stop_word` list that Tidytext provides:


```R
folkNotes <- folkNotes %>%
  anti_join(stop_words)

# an "anti_join" means that 'stop_words' is joined to the data you load in ('folkNotes') and any time a word matches in both datasets, it's removed.

head(folkNotes)
```

There we go! At this point, we can calculate the word frequencies of what is written in `folk_notes` like we did earlier (notice anything similar to what we saw in motifs?):


```R
folkNotesFreq <- folkNotes %>%
    count(word, sort = TRUE)

# show it ALL
folkNotesFreq
```

## Filtering

Filtering is exactly what it sounds like; it allows you to filter your data based on a specified condition. Here's 2 examples of how it may be used with the data we just generated:


```R
# library(dplyr)

# only show the words that occur more than 50 times
folkNotes_gt <- folkNotesFreq %>%
  filter(n > 50)

print("Words which occur >50 times: ")
folkNotes_gt

# only show words that contain 'tree'-- uses str_detect() from stringr!
folkNotes_trees <- folkNotesFreq %>%
  filter(str_detect(word, "tree"))

print("Trees found in folk notes: ")
folkNotes_trees
```

## Merging Data

Finally, let's say you have 2 or more separate files with data that you think may be useful to analyze collectively. You can do that easily by loading in each dataset, and then using the `full_join()` function to bind them all together. For example, I will now join another dataset I have on the devil's appearence pulled from the same databasse as `fairyElements`:


```R
# library(dplyr)

devilAppearence <- read_csv("devilappearence.csv")

# adding an id-- IF YOUR DATASETS DON'T HAVE A MATCHING COLUMN (ex "name" or "date") don't just add an ID, first use 'bind_rows()', which works the same way as 'full_join()'
devilAppearence$id <- 1:nrow(devilAppearence)

unnaturalBeingsInfo <- full_join(fairyElements, devilAppearence, by = "id")

unnaturalBeingsInfo
```

And they're combined! But, as you may have noticed, the data from `devilAppearence` isn't clean! My challenge to you is to use what you now know and modify the cell above to clean `devilAppearence` before binding it! When build this tutorial, I referenced:

- The [Tidytext documentation](https://www.rdocumentation.org/packages/tidytext/versions/0.2.6)
- The [Tidyverse documentation](https://www.tidyverse.org/packages/)
- [This article](https://sebastiansauer.github.io/dplyr_filter/) with more examples of how to use `filter()`

If you want to download your data at anytime during this process, you can run:


```R
# library(readr)

# downloading the frame of words that occur more than 50 times in folk_notes
# this will save in the directory that you are working in!
write_csv(folkNotes_gt, "folkNotes_gt.csv")
```

**"What do I do if I get stuck?"**

If none of those references help, I get it! Programming can be confusing-- and so can googling about it. If you want to search for a solution to an issue you're running into, try following this format:

`R [package you're using] [issue/error message you're getting]`

You of course can also ask your peers for help in the course Discord-- teamwork makes the dream work! Good luck!
