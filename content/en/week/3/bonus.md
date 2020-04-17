---
title: "This week's bonus"
description: "Network analysis in R"
date: 2020-01-28T00:10:48+09:00
draft: false
weight: -1
---

If you're interested in other network tools or data cleaning tools, drop me a line. Many folks use R and R Studio and its various network analysis packages, which have the edge over Gephi in that we can write the script and share that, rather than long, tortured step-by-step 'click this' and 'click that' type instructions.

Ah, what the hang, here we go.

Grab this data:

[List of links (this downloads a CSV file)](http://workbook.craftingdigitalhistory.ca/supporting%20materials/texaslinks.csv)
[List of nodes(this downloads a CSV file)](http://workbook.craftingdigitalhistory.ca/supporting%20materials/texasnodes.csv)


Fire up R Studio, make a new script, run each line one at a time:

```R
# install igraph; this might take a long time
# you only run this line the first time you install igraph:
install.packages('igraph')

# a lot of stuff gets downloaded and installed.

# now tell RStudio you want to use the igraph pacakge and its functions:
library('igraph')

# now let's load up the data by putting the csv files into nodes and links.
nodes <- read.csv("texasnodes.csv", header=T, as.is=T)
links <- read.csv("texaslinks.csv", header=T, as.is=T)

#examine data
head(nodes)
head(links)

nrow(nodes); length(unique(nodes$id))
# which gives the number of nodes in our data

nrow(links); nrow(unique(links[,c("source", "target")]))
# which gives the number of sources, and number of targets
# which means some people sent more than one letter, and some people received more than one letter

links <- aggregate(links[,3], links[,-3], sum)

links <- links[order(links$target, links$source),]

colnames(links)[3] <- "weight"

rownames(links) <- NULL

head(links)

# let's make a net
# notice that we are telling igraph that the network is directed, that the relationship Alice to Bob is different than Bob's to Alice (Alice is the _sender_, and Bob is the _receiver_)

# In older DH Box version of igraph in RStudio:
net <- graph.data.frame(d=links, vertices=nodes, directed=T)

# OR Newer version of igraph in desktop RStudio:
net <- graph_from_data_frame(d=links, vertices=nodes, directed=T)

# type 'net' again and run the line to see how the network is represented.
net

# let's visualizae it
plot(net, edge.arrow.size=.4,vertex.label=NA)

# two quite distinct groupings, it would appear.
---

Before we jump down the rabbit hole of visualization, let's recognize right now that visualizing a network is only rarely of analytical value. The value of network analysis comes from the various questions we can now start posing of our data when it is represented as a network. In this correspondence network, who is in the centre of the web? To whom would information flow? To whom would information leak? Are there cliques or ingroups? When we identify such individuals, how does that confirm or confound our expectations of the period and place?

Many different kinds of metrics can be calculated (and the [Kateto R igraph tutorial](https://kateto.net/networks-r-igraph) will show you how) but it's always worth remembering that a metric is only meaningful for a given network when we're dealing with similar things — a network of people who write letters to one another; a network of banks that swap mortgages with one another. These are called 'one mode networks'. A network of people connected to the banks they use — a two mode network, because it connects two different kinds of things — might be useful to visualize but the metrics calculated might not be valid if the metric was designed to work on a one-mode network .

Given our correspondence network, let's imagine that 'closeness' (a measure of how central a person is) and 'betweenness' (a measure of how many different strands of the network pass through this person) are the most historically interesting. Further, we're going to try to determine if there are subgroups in our network, cliques.

```R
## the 'degree' of a node is the count of its connections. In this code chunk, we calculate degree, then make both a histogram of the counts and a plot of the network where we size the nodes proportionately to their degree. What do we learn from these two visualizations?
deg <- degree(net, mode="all")
hist(deg, breaks=1:vcount(net)-1, main="Histogram of node degree")
plot(net, vertex.size=deg*2, vertex.label = NA)
## write this info to file for safekeeping
write.csv(deg, 'degree.csv')
```

Now we'll look at closeness. If you know the width or diameter of your network (the maximum number of steps to get across it), then the node that is on average the shortest number of steps from all the others is the one that is closest. We calculate it like the following:

```R
closepeople <- closeness(net, mode="all", weights=NA)
sort(closepeople, decreasing = T) # so that we see who is most close first
write.csv(closepeople, 'closeness.csv') # so we have it on file.
```
We can ask which individuals are hubs, and which are authorities. In the lingo, 'hubs' are individuals with many outgoing links (they sent lots of letters) while 'authorities' are individuals who received lots of letters. In the code below, can you work out what command to give to write the hub score or the authority scores to a file?

```R
hs <- hub_score(net, weights=NA)$vector
as <- authority_score(net, weights=NA)$vector

par(mfrow=c(1,2))

# vertex.label.cex sets the size of the label; play with the sizes until you see something appealing.
plot(net, vertex.size=hs*40, vertex.label.cex =.2, edge.arrow.size=.1, main="Hubs")
plot(net, vertex.size=as*20, vertex.label = NA, edge.arrow.size=.1, main="Authorities")
```
Let's look for 'modules' within this network. Broadly speaking, these are clumps of nodes that have more or less the same pattern of ties between them, within the group, than without.

```R
cfg <- cluster_fast_greedy(as.undirected(net))

lapply(cfg, function(x) write.table( data.frame(x), 'cfg.csv'  , append= T, sep=',' ))
```
We create a new variable called `cfg` and get the `cluster_fast_greedy` algorithm to perform its calculations. The next line writes the groups to a file, separating each group with an `x`. (If you tried `write.csv` as before, you'll get an error message because the output of the algorithm gives a different kind of data type. R is fussy like this.) Examine that file — what groups do you spot? What might these mean, if you went back to the content of the original letters?

The line below will plot out our network, colouring it by the communities discerned above, like the following:

```R
plot(cfg, net, vertex.size = 1, vertex.label.cex =.2, edge.arrow.size=.1, main="Communities")
```

You can export the plot by clicking on the 'Export' button in the plot panel, to PDF or to PNG. But this is a pretty ugly network. We need to apply a layout to try to make it more visually understandable. There are many different layout options in igraph. We'll assign the layout we want to a variable, and then we'll give that variable to the `plot` command like the following:

```R
l1 <- layout_with_fr(net)

plot(cfg, net, layout=l1, vertex.size = 1, vertex.label.cex =.2, edge.arrow.size=.1, main="Communities")
```
The `layout_with_fr` is calling the 'Fruchterman-Reingold' algorithm, a kind of layout that imagines each node as a repulsive power, pushing against all the other nodes (it's part of a family of layouts called 'Forced Atlas'). That also means that if you ran the plot command a couple of times, the nodes might end up in different places each time — they are jostling for relative position, and this positioning itself carries no meaning in and of itself (so if a node is at the top of the screen, or the centre of the screen, don't read that to mean anything about importance).

Good luck! Remember to save your script, and upload the entire project folder to your GitHub repository.
