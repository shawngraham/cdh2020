---
title: "Network Analysis in Python"
description: "The networkx package"
date: 2020-01-28T00:10:48+09:00
draft: false
---

_This tutorial follows Melanie Walsh's Networks tutorial from her course [Introduction to Cultural Analytics](https://melaniewalsh.github.io/Intro-Cultural-Analytics/Network-Analysis/Network-Analysis.html) with some minor modifications._

Network analysis is all about looking at the patterns of _relationships_ we find in our data, and seeing how individual 'nodes' are positioned vis-a-vis all the other ones, or what the emergent, macroscopic patterns might imply about how the entire population of ...whatever it is you're studying... might experience things like group identity or flows of information. In this tutorial, we're looking at a network of correspondence between important figures in the Republic of Texas.

Grab this data (which is derived from the data first encountered in the [regex tutorial](/tutorials/regex) and then further cleaned in the [open refine tutorial](/tutorials/open-refine)):

[List of links (this downloads a CSV file)](http://workbook.craftingdigitalhistory.ca/supporting%20materials/texaslinks.csv)
[List of nodes(this downloads a CSV file)](http://workbook.craftingdigitalhistory.ca/supporting%20materials/texasnodes.csv)

## Fire up your notebook

Start a new Jupyter notebook (you can use the Anaconda Navigator to start one up).

First, we're going to install the libraries that we need:

```Python
!pip install networkx
!pip install pandas
```
Then we'll import them, along with matplotlib so we can make nice diagrams:

```Python
import networkx
import pandas as pd
pd.set_option('max_rows', 400)
import matplotlib.pyplot as plt
```

(You might need to `!pip install` matplotlib too, depending on your setup.)

Now we'll load up the data describing our network of letter-writers. The nodes are the people who sent and received letters; the links are the letters that connect them. Make sure the `texaslinks.csv` and `texasnodes.csv` are in the same folder as your notebook .ipynb file; or you can read them directly off the web:

```Python
texas_df = pd.read_csv('http://workbook.craftingdigitalhistory.ca/supporting%20materials/texaslinks.csv')

# take a look
texas_df
```

Now let's build a network from that data:
```Python
G = networkx.from_pandas_edgelist(texas_df, 'source', 'target', 'weight')

# and then we'll draw it!

networkx.draw(G)
```

Let's see if we can make that a bit more readable:

```python
plt.figure(figsize=(8,8))
networkx.draw(G, with_labels=True, node_color='skyblue', width=.3, font_size=8)
```

That's a bit better. Try fiddling with the figsize, the width, and the fontsize.

## Network metrics

By looking at how different nodes connect or not, we can begin to examine how individuals were in positions to control information (for instance) or we might ask, are there any subgroups implied by these connections? The simplest indication of importance in a network might be 'degree' or the number of connections a node has. (And, historically, what might being a prolific letter writer or receiver-of-letters imply for a politician in the Republic of Texas? Always try to imagine what these metrics actually imply.)

We can calculate degree like so:

```Python
networkx.degree(G)
```

...but the result is a bit hard to read. We'll convert it to a kind of list and then add the result as another attribute of the individual nodes in the graph:

```python
degrees = dict(networkx.degree(G))
networkx.set_node_attributes(G, name='degree', values=degrees)
```

Then it'd be nice to actually have this information in our actual dataframe or table:

```Python
degree_df = pd.DataFrame(G.nodes(data='degree'), columns=['node', 'degree'])
degree_df = degree_df.sort_values(by='degree', ascending=False)
degree_df
```

(Now, if you want to write your graph to file, you'd run this little snippet: ```networkx.write_graphml(G, 'Texas-network.graphml')```. You can open a graphml file using Gephi to make it pretty (see this [help with gephi](/tutorials/networks).)

We can make a graph of the individuals with the highest degree values:

```python
num_nodes_to_inspect = 10
degree_df[:num_nodes_to_inspect].plot(x='node', y='degree', kind='barh').invert_yaxis()
```

We might ask, who is _most central_? And by 'most central', we mean, who is on the most shortest paths between any two correspondents? Such a person might be in a prime position to influence or control information flow.

```Python
networkx.betweenness_centrality(G)
```

(compare that command with the one for 'degree'. What changed? You can google the networkx package for other metrics to calculate, and you'd form the command much the same way.)

Let's add it to our data frame, same as we did before:

```python
betweenness_centrality = networkx.betweenness_centrality(G)
networkx.set_node_attributes(G, name='betweenness', values=betweenness_centrality)
betweenness_df = pd.DataFrame(G.nodes(data='betweenness'), columns=['node', 'betweenness'])
betweenness_df = betweenness_df.sort_values(by='betweenness', ascending=False)
betweenness_df
```

Let's plot the highest betweeness values:

```python
num_nodes_to_inspect = 10
betweenness_df[:num_nodes_to_inspect].plot(x='node', y='betweenness', color='green', kind='barh').invert_yaxis()
```

How do the results change? What might this mean, historically?

Let's see if there are any 'communities' implied by the network.

First we get the piece of code we want from networkx, then we use a particular community detection routine to fine them, then we'll print them out:

```python
from networkx.algorithms import community
communities = community.greedy_modularity_communities(G)
communities
```

Then we'll add this information into our graph:

```python
# Create empty dictionary
modularity_class = {}
#Loop through each community in the network
for community_number, community in enumerate(communities):
    #For each member of the community, add their community number
    for name in community:
        modularity_class[name] = community_number

# now we add that to the network as an attribute
networkx.set_node_attributes(G, modularity_class, 'modularity_class')
```
then we'll add it also to our dataframe:

```python
communities_df = pd.DataFrame(G.nodes(data='modularity_class'), columns=['node', 'modularity_class'])
communities_df = communities_df.sort_values(by='modularity_class', ascending=False)
communities_df
```

Then, if you want to see who is a member of a particular group, you can select it like so:

```Python
communities_df[communities_df['modularity_class'] == 4]
```

...where you change the numeral to whichever group you want. You might want to see who is in what group by making a plot:

```python
import seaborn as sns
#Set figure size
plt.figure(figsize=(4,12))

#Plot a categorical scatter plot from the dataframe communities_df.sample(40)
# if you want more or fewer individuals, change the '40'
ax =sns.stripplot(x='modularity_class', y='node', data=communities_df.sample(40),
              hue='modularity_class', marker='*',size=15)
#Set legend outside the plot with bbox_to_anchor
ax.legend(loc='upper right',bbox_to_anchor=(1.5, 1), title='Modularity Class')
ax.set_title("Figures in the Republic of Texas Correspondence by Modularity Class\n(Random Sample)")
plt.show()
```

Finally, get all of your metrics into a nice table:

```Python
dict(G.nodes(data=True))
nodes_df = pd.DataFrame(dict(G.nodes(data=True))).T
nodes_df
```

You can sort it like so: ```nodes_df.sort_values(by='betweenness', ascending=False)```

## Go Even Further

Try adapting Melanie Walsh's code for an interactive network visualization built on Game of Thrones characters for your Texas data! [Follow this link](https://melaniewalsh.github.io/Intro-Cultural-Analytics/Network-Analysis/Making-Network-Viz-with-Bokeh.html).
