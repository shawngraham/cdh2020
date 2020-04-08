---
title: "APIs"
description: "instructions"
date: 2020-01-28T00:10:51+09:00
draft: false
weight: -2
author: "Shawn Graham"
---

Sometimes, a website will have what is called an Application Programming Interface or API. In essence, this lets a program on your computer talk to the computer serving the website you're interested in, such that the website gives you the data that you're looking for.

An API is just a way of opening up a program so that another program can interact with it. That is, instead of an interface meant for a human to interact with the machine, there’s an API to allow some other machine to interact with this one. If you’ve ever downloaded an app on your phone that allowed you to interact with Instagram (but wasn’t Instagram itself), that interaction was through Instagram’s API, for instance.

A good API will also have documentation explaining what or how to make calls to it to get the information you want. That is, instead of you punching in the search terms on a search page, and copying and pasting the results, you frame a request as a URL. More or less. The results often come back to you in a text format where data is organized according to keys and values (JSON). Sometimes it's just a table of data using commas to separate each field for each row (.csv file). JSON looks like the following:

![apis](https://i.imgur.com/LtZWyle.png)

### Getting material out of an API

Each API has its own idiosyncracies, but we can always look at the documentation and figure out how to form our requests so that our programs grab the data we're after. The [Chronicling America website](https://chroniclingamerica.loc.gov/) from the Library of Congress has digitized American newspapers from 1789 to 1963. While the site has a fine search interface, we'll use the API to grab *every* article that mentions the word 'archeology' (sic).

First of all, go to the Cronicling America site and [search in the text box for `archeology`](https://chroniclingamerica.loc.gov/search/pages/results/?state=&date1=1789&date2=1963&proxtext=archeology&x=0&y=0&dateFilterType=yearRange&rows=20&searchType=basic). Notice how the url changes when it brings back the results:

`https://chroniclingamerica.loc.gov/search/pages/results/?state=&date1=1789&date2=1963&proxtext=archeology&x=0&y=0&dateFilterType=yearRange&rows=20&searchType=basic`

There's a lot of stuff in there - amongst other things, we can see a setting for `state`, for `date1` and `date2`, our search term appears as the value for a setting called `proxtext`. This is the API in action.

Let's build.

1. Open a new file in Sublime Text. We'll first put a bit of metadata in our file, for the programme we're going to write:

```python
#!/usr/bin/env python
"""
a script for getting materials from the Chronicling America website
"""

# Make these modules available
import requests
import json

__author__ = "your-name"
```

The first bit tells us this is a python file. The next bit tells us what the file is for. The `import` tells python that we'll need a module called `requests` which lets us grab materials from the web, and `json` which helps us deal with json formatted data. The final bit says who wrote the script.

2. Now we're going to define some variables to hold the bit of the search url up to where the `?` occurs - everything after the question mark are the parameters we want the API to search. We create the api_searh_url, define the parameter we want to search for, and define how we want the results returned to us. Add the following to your script:

```python
# Create a variable called 'api_search_url' and give it a value
api_search_url = 'https://chroniclingamerica.loc.gov/search/pages/results/'

# This creates a dictionary called 'params' and sets values for the API's mandatory parameters
params = {
    'proxtext': 'archeology' # Search for this keyword     
}

# This adds a value for 'encoding' to our dictionary
params['format'] = 'json'
```

3. Now we'll send the request to the server, and we'll add a bit of error checking so that if something is wrong, we'll get some indication of why that is. Add this to your script:

```python
# This sends our request to the API and stores the result in a variable called 'response'
response = requests.get(api_search_url, params=params)

# This shows us the url that's sent to the API
print('Here\'s the formatted url that gets sent to the ChronAmerca API:\n{}\n'.format(response.url))

# This checks the status code of the response to make sure there were no errors
if response.status_code == requests.codes.ok:
    print('All ok')
elif response.status_code == 403:
    print('There was an authentication error. Did you paste your API above?')
else:
    print('There was a problem. Error code: {}'.format(response.status_code))
```
4. Now let's get the results, and put them into a variable called 'data'. Then we'll print the results to the terminal, and finish by also writing the results to a file.

```python
# Get the API's JSON results and make them available as a Python variable called 'data'
data = response.json()

# Let's prettify the raw JSON data and then display it.

# We're using the Pygments library to add some colour to the output, so we need to import it

from pygments import highlight, lexers, formatters

# This uses Python's JSON module to output the results as nicely indented text
formatted_data = json.dumps(data, indent=2)

# This colours the text
highlighted_data = highlight(formatted_data, lexers.JsonLexer(), formatters.TerminalFormatter())

# And now display the results
print(highlighted_data)

# dump json to file
with open('data.json', 'w') as outfile:
    json.dump(data, outfile)
```

Save your file as `ca.py`. Open a terminal/command prompt (remember Windows folks: anaconda powershell!) in the folder where you saved this file, and run it with:

`$ python ca.py`

Your terminal will look like it's frozen for a few moments; that's because your computer is reaching out to the Chronicling America website, making its request, and pulling down the results. But in seconds, you'll have a `data.json` file with loads of data - 9021 articles in fact!

Congratulations, you now have a program that you wrote that you can use to obtain all sorts of historical information.

Your complete file will look like this:

```Python
#!/usr/bin/env python
"""
a script for getting materials from the Chronicling America website
"""

# Make these modules available
import requests
import json

__author__ = "your-name"

# Create a variable called 'api_search_url' and give it a value
api_search_url = 'https://chroniclingamerica.loc.gov/search/pages/results/'

# This creates a dictionary called 'params' and sets values for the API's mandatory parameters
params = {
    'proxtext': 'archeology' # Search for this keyword
}

# This adds a value for 'encoding' to our dictionary
params['format'] = 'json'

# This sends our request to the API and stores the result in a variable called 'response'
response = requests.get(api_search_url, params=params)

# This shows us the url that's sent to the API
print('Here\'s the formatted url that gets sent to the ChronAmerca API:\n{}\n'.format(response.url))

# This checks the status code of the response to make sure there were no errors
if response.status_code == requests.codes.ok:
    print('All ok')
elif response.status_code == 403:
    print('There was an authentication error. Did you paste your API above?')
else:
    print('There was a problem. Error code: {}'.format(response.status_code))

# Get the API's JSON results and make them available as a Python variable called 'data'
data = response.json()

# Let's prettify the raw JSON data and then display it.
# We're using the Pygments library to add some colour to the output, so we need to import it

from pygments import highlight, lexers, formatters

# This uses Python's JSON module to output the results as nicely indented text
formatted_data = json.dumps(data, indent=2)

# This colours the text
highlighted_data = highlight(formatted_data, lexers.JsonLexer(), formatters.TerminalFormatter())

# And now display the results
print(highlighted_data)

# dump json to file
with open('data.json', 'w') as outfile:
    json.dump(data, outfile)
```
### Some other APIs

You can modify this code to extract information from other APIs, but it takes a bit of tinkering. In essence, you need to study the website to see how they form the API, and then change up lines 13, 17 and 21 accordingly. You can see this in action for instance [here, with regard to the Metropolitan Museum of Art](https://github.com/o-date/working-with-apis/blob/master/notebooks/metropolitan%20museum%20of%20art%20API.ipynb) or [here, with regard to the Smithsonian](https://github.com/o-date/working-with-apis/blob/master/notebooks/retrieving%20data%20from%20the%20Smithsonian%20OA%20api.ipynb).  

### But... it's in json format?

JSON is handy for lots of computational tasks, but for you as a beginning digital historian, you might want to have the data as a table. There are a couple of options here. The easiest right now - and there's no shame in doing this - is to use an online converter. This site: [json-csv.com](http://json-csv.com) lets you convert your json file to csv or Excel spreadsheet, and even transfer it over to a google doc. Give that a shot right now; the text of the articles by the way is in the field 'ocr_eng' which tells us that the text was originally transcribed from the images using object character recognition - so there will be errors and weird glitches in the text. Fortunately, there's also a URL with the direct link to the original document, so you can check things for yourself.
