---
title: "Excel & R"
description: "Spreadsheets & Scripting"
date: 2020-01-28T00:10:37+09:00
draft: false
weight: -5
---
{{< notice warning "An Assumption" >}}
I might be making a terrible assumption, but I'm assuming that many if not most of you have played around with Excel before. What follows is a very basic introduction, just in case.
{{< /notice >}}

Library and Archives Canada provides [links to historical census data](https://www.bac-lac.gc.ca/eng/census/Pages/census.aspx), along with quite comprehensive discussion about how the material was digitized, how it was collected in the first place, and the limitations of the data.

Unfortunately, none of it is available as a simple table that we could download. Indeed, if you go through that link and look, you'll see _just how much work_ would be involved in trying to digitize these things so that we could do quantitative work on it!

So for the purposes of becoming acquainted with Excel, let's download some data from the [Canadian 2016 Census](https://www12.statcan.gc.ca/census-recensement/2016/dp-pd/dt-td/Rp-eng.cfm?TABID=2&LANG=E&A=R&APATH=3&DETAIL=0&DIM=0&FL=A&FREE=0&GC=01&GL=-1&GID=1235625&GK=1&GRP=1&O=D&PID=109523&PRID=10&PTYPE=109445&S=0&SHOWALL=0&SUB=0&Temporal=2016&THEME=115&VID=0&VNAMEE=&VNAMEF=&D1=0&D2=0&D3=0&D4=0&D5=0&D6=0) - right-click and save-as: ['Age (in Single Years) and Average Age (127) and Sex (3) for the Population of Canada, Provinces and Territories, Census Metropolitan Areas and Census Agglomerations, 2016 adn 2011 Censuses - 100% Data](https://www12.statcan.gc.ca/census-recensement/2016/dp-pd/dt-td/CompDataDownload.cfm?LANG=E&PID=109523&OFT=CSV)

Unzip that file; inside you'll find the data files and files describing the data, which we call the 'metadata'.

Now, Excel is a very powerful tool. It also comes in several different flavours with slightly different layouts, which makes _teaching_ excel extremely vexing. Most new PCs will have Excel on them; most new Macs will not. Do not worry if you don't have a copy of Excel; Google Sheets also works in a very similar vein.

The key idea is that each **cell** in a spreadsheet as an **address**. Knowing the address for a **range** of cells, or just one cell in particular means that you can do calculations that update themselves as you change the data. For instance, if I had all of my student's scores in say column A from a test (not that I assign tests), I could have a cell at the bottom of that column that has this **formula**: `=average(a1:a23)`. Excel will tally up all of the cells from a1 to a23 (column a, row 1, to column a, row 23), and divide that sum by the count of cells, printing out an average. If I made a mistake on a student's score, I could change it (by re-entering the score in say cell a13) and Excel will automatically adjust the average.

You can thus program pretty nearly any mathematical operation you want into a spreadsheet, remembering that cells don't just hold values, they can also hold formulae.

1. Start Excel. Under 'open', find the csv that you downloaded. Excel should be able to import this csv without issue; older versions of Excel sometimes have an issue here, and so will need to turn on the Text Import Wizard, [details here](https://support.office.com/en-us/article/text-import-wizard-c5b02af6-fda1-4440-899f-f78bafe41857).

2. What a lovely table of data. Let's filter it for just the results for the Ontario side of the Ottawa-Gatineau census metropolitan area.

![filter button](/images/excel/filter-button.png)
What the filter button looks like on my version of Excel

Every column now has a little drop-down arrow on it. Find column D, 'GEO_NAME' and press it:
![filter options](/images/excel/filter-options.png)
Uncheck 'select all' and scroll down through that until you see 'Ottawa-Gatineau (Ontario Part)'

Boom! Your spreadsheet is now displaying 254 of 44196 records.

2. Now, each row in this spreadsheet is an age category. We're going to write a little formula to add up all of the female children 1 to 4 years of age. (This data starts in Row 15500!). Can you spot that data?

![here it is](/images/excel/select-cells.png)

When you highlight cells, Excel automatically does a few calculations like averaging, counting, and summing them, which you can spot at the bottom.

In cell R15500, let's write a little formula. We want to get the sum of female children 1 to 4 years of age. Our formula is going to look like this `=sum()` and _between_ the parentheses will be the range. Put in the correct range, and hit enter. Did you get the right value? Have you selected the correct cells?

3. Let's make a chart. Using your mouse, click and drag down the Age column so that you select 1,2,3,4. Then, holding down the command key (mac) or ctrl (pc) click and drag again on the counts. You end up with two columns highlighted:

![two columns](/images/excel/two-columns.png)

Then, click 'insert', then 'recommended charts' and pick one of the charts. Boom, instant chart. The first row we highlighted gives us our x value, the second our y; you can right-click on a chart to open up some interfaces for changing those assumptions, for adding labels, and so on.

![chart](/images/excel/chart.png)  

When you save your work in Excel, an Excel file can contain numerous sheets, charts, interlinkages, and other quite complex objects. If you 'save as' and select 'csv', you'll _only_ get the data in the spreadsheet sheet currently active - plus a whole bunch of end-of-the-world warnings from Excel.

4. Pivot Tables
Pivot tables are a very useful feature of Excel; they are a way of summarizing quite complex tables and visualizing the summaries. But I won't discuss them now. Instead, once you do the Topic Modeling Tool walk through, check out the final section in the official documentation on 'Build a pivot table' ([here](https://senderle.github.io/topic-modeling-tool/documentation/2017/01/06/quickstart.html)).

I'm not going to invest much time in Excel; but here's some [more guidance from Microsoft itself.](https://support.office.com/en-us/article/Basic-tasks-in-Excel-dc775dd1-fa52-430f-9c3c-d998d1735fca)

### Some Basic Counting and Plotting in R  

Start up your RStudio from Anaconda Navigator, and make a new R script. The first thing we're going to do is get set up so that we can import some data directly from the web. We use the `RCurl` package to do that:

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
