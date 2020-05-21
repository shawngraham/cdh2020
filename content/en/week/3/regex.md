---
title: "REGEX"
description: "Learning how to pattern match"
date: 2020-01-28T00:10:48+09:00
draft: false
weight: -4
---

### Introduction

A regular expression (also called regex) is a powerful tool for finding and manipulating text. At its simplest, a regular expression is just a way of looking through texts to locate _patterns_. When you search a website for instance (use ctrl+f in most applications to search, by the way), the search box finds exact matches; there's no room for fuzziness. A regular expression on the other hand can help you find every line that begins with a number, or every instance of an email address, or whenever a word is used even if there are slight variations in how it's spelled. As long as you can describe the pattern you're looking for, regular expressions can help you find it. Once you've found your patterns, they can then help you manipulate your text so that it fits just what you need.

Regular expressions can look pretty complex, but once you know the basic syntax and vocabulary, simple ‘regexes’ will be easy. Regular expressions can often be used right inside the 'Find and Replace' box in many text and document editors, such as Sublime Text, Atom, or Notepad++. You cannot use Microsoft Word, however!

NB In text editors, you have to indicate that you wish to do a regex search. In Sublime Text, you open the search panel from the 'Find' menu (or use the shortcut) and then you need to tick the box that has `.*` in the search panel to enable regular expression searches.

### Some basic principles

**Protip**: there are libraries of regular expressions, online. For example, if you want to find all postal codes, you can search “regular expression Canadian postal code” and learn what ‘formula’ to search for to find them.

Also, here is a cheatsheet for [regular expressions in Sublime Text](https://jdhao.github.io/2019/02/28/sublime_text_regex_cheat_sheet/).

Let's say you're looking for all the instances of "cat" or "dog" in your document. When you type the vertical bar on your keyboard (it looks like `|`, shift+backslash on windows keyboards), that means 'or' in regular expressions. So, if your query is `dog|cat` and you press 'find', it will show you the first time either dog or cat appears in your text.

If you want to replace every instance of either "cat" or "dog" in your document with the world "animal", you would open your find-and-replace box, put dog|cat in the search query, put animal in the 'replace' box, hit 'replace all', and watch your entire document fill up with references to animals instead of dogs and cats.

The astute reader will have noticed a problem with the instructions above; simply replacing every instance of "dog" or "cat" with "animal" is bound to create problems. Simple searches don't differentiate between letters and spaces, so every time "cat" or "dog" appear within words, they'll also be replaced with "animal". "catch" will become "animalch"; "dogma" will become "animalma"; "certificate" will become "certifianimale". In this case, the solution appears simple; put a space before and after your search query, so now it reads:

`dog | cat`

With the spaces, "animal" replace "dog" or "cat" only in those instances where they're definitely complete words; that is, when they're separated by spaces.

The even more astute reader will notice that this still does not solve our problem of replacing every instance of "dog" or "cat". What if the word comes at the beginning of a line, so it is not in front of a space? What if the word is at the end of a sentence or a clause, and thus followed by a punctuation? Luckily, in the language of regex, you can represent the beginning or end of a word using special characters.

`\<`

means the beginning of a word. In some programs, you might use this alternative:

`\b`

so if you search for `\<cat` , (or, as it may be, `\bcat` )it will find "cat", "catch", and "catsup", but not "copycat", because your query searched for words beginning with "cat". For patterns at the end of the line, you would use:

`\>` or `\b`

again. If you therefore searched:

`cat\>`

it will find "cat" and "copycat", but not "catch," because your query searched for words ending with -"cat".

Regular expressions can be mixed, so if you wanted to find words only matching "cat", no matter where in the sentence, you'd search for

`\<cat\>`

which would find every instance. And, because all regular expressions can be mixed, if you searched for

`\<cat|dog\>`

and replaced all with "animal", you would have a document that replaced all instances of "dog" or "cat" with "animal", no matter where in the sentence they appear. You can also search for variations within a single word using parentheses. For example if you were looking for instances of "gray" or "grey", instead of the search query

`gray|grey`

you could type

`gr(a|e)y`

instead. The parentheses signify a group, and like the order of operations in arithmetic, regular expressions read the parentheses before anything else. Similarly, if you wanted to find instances of either "that dog" or "that cat", you would search for:

`(that dog)|(that cat)`

Notice that the vertical bar `|` can appear either inside or outside the parentheses, depending on what you want to search for.

The period character `.` in regular expressions directs the search to just find any character at all. For example, if we searched for:

`d.g`

the search would return "dig", "dog", "dug", and so forth.

Another special character from our cheat sheet, the plus symbol `+` instructs the program to find any number of the previous character. If we search for

`do+g`

it would return any words that looked like "dog", "doog", "dooog", and so forth. Adding parentheses before the plus would make a search for repetitions of whatever is in the parentheses, for example querying

`(do)+g`

would return "dog", "dodog", "dododog", and so forth.

Combining the plus `+` and period `.` characters can be particularly powerful in regular expressions, instructing the program to find any amount of any characters within your search. A search for

`d.+g`

for example, might return "dried fruits are g", because the string begins with "d" and ends with "g", and has various characters in the middle. Searching for simply `.+` will yield query results that are entire lines of text, because you are searching for any character, and any amount of them.

Parentheses in regular expressions are also very useful when replacing text. The text within a regular expression forms what's called a group, and the software you use to search remembers which groups you queried in order of their appearance. For example, if you search for

`(dogs)( and )(cats)`

which would find all instances of "dogs and cats" in your document, your program would remember "dogs" is group 1, "and" is group 2, and "cats" is group 3. Sublime Text remembers them as `\1`, `\2`, and `\3` for each group respectively.

If you wanted to switch the order of "dogs" and "cats" every time the phrase "dogs and cats" appeared in your document, you would type

`(dogs)( and )(cats)`

in the 'find' box, and

`\3\2\1`

in the 'replace' box. That would replace the entire string with group 3 ("cats") in the first spot, group 2 (" and ") in the second spot, and group 1 ("dogs") in the last spot, thus changing the result to "cats and dogs".

The vocabulary of regular expressions is pretty large, but there are many cheat sheets for regex online.

### Letters of the Republic of Texas

The correspondence of the [Republic of Texas](https://en.wikipedia.org/wiki/Republic_of_Texas), and independent state from 1835 to 1846 was collated into a single volume and published with a helpful index in 1911. It was scanned and OCR'd by Google, and is now available as a text file from the Internet Archive. You can see the OCR'd text at [archive.org](http://archive.org/stream/diplomaticcorre33statgoog/diplomaticcorre33statgoog_djvu.txt). We are going to use it to practice our regex skills because it is an example of a historical network we might want to analyze later with network analysis. We are going to grab the index from that file, and transform it using regex.

Entries in the index look like this:

```
Sam Houston to A. B. Roman, September 12, 1842 101
Sam Houston to A. B. Roman, October 29, 1842 101
Correspondence for 1843-1846 —
Isaac Van Zandt to Anson Jones, January 11, 1843 103
```

We are going to use regex to tidy this up, delete some parts, and end up with data that looks like this:

```
Sam Houston, A. B. Roman, September 12 1842
Sam Houston, A. B. Roman, October 29 1842
Isaac Van Zandt, Anson Jones, January 11 1843
```
The change doesn't look like much, and you might think to yourself, 'hey, I could just do that by hand'. You could but it'd take you ages, and if you made a mistake somewhere, are you sure you could do this consistently, for a couple of hours at a time? Probably not. Your time is better spent figuring out the search and replace patterns, and then setting your machine loose to implement it.

1. We will use the `curl` command at the command prompt to grab the file; this is a related command to wget whom you've already met. Mac users should already have this installed; Windows users can open up Anaconda PowerShell and type `conda install -c anaconda curl` to get it.

2. At the command line, type the following curl command:

`$ curl http://archive.org/stream/diplomaticcorre33statgoog/diplomaticcorre33statgoog_djvu.txt > texas.txt`

The `curl` command downloads the txt file and the the `>` pushes the result of the command to a file called `texas.txt`.

3. Open `texas.txt` in Sublime Text, and open the Find menu and hit the Replace option; this opens the find and replace panel at the bottom of your window. Make sure to press the `.*` button in that panel to turn on regular expression search.

4. Delete everything in this file that **isn't** the table of letters. The table starts with ‘Sam Houston to J. Pinckney Henderson, December 31, 1836 51’ and ends with ‘Wm. Henry Daingerfield to Ebenezer Allen, February 2, 1846 1582’. Your file will now have approximately 2000 lines in it.

5. First thing we're going to do is identify any lines that indicate correspondence. We're going to look for the word 'to'. In fact, we don't just want to find "to", but the entire line that contains it. We assume that every line that contains the word "to" in full is a line that has relevant letter information, and every line that does not is one we do not need.

You learned earlier that the pattern `.+` returns any amount of text, no matter what it says, and that `\b` indicates a word boundary. Thus, the pattern we want looks like this `.+\bto\b.+`. **Don't search yet**.

We want to mark these lines off as special, so let's add a tilde ~ before each of the lines that look like letters. This involves the find-and-replace function, and a query identical to the one before, but with parentheses around it, so it looks like the following

(.+\<to\>)

and the entire line is placed within a parenthetical group. Since this the first group in our search expression, we can replace that group with `\1` and put the tilde in front of it like so: `~\1`.

**Do this**
search for: `(.+\<to\>)`

replace with: `~\1`

6. After running the find-and-replace, you should note your document now has most of the lines with tildes in front of it, and a few which do not. The next step is to remove all the lines that do not include a tilde. The search string to find all lines which don't begin with tildes is `\n[^~.+]`

Within a set of square brackets `[]` the carrot `^` means search for anything that **isn't** within these brackets (in this case, the tilde ~). The `+` as before means search for every remaining character in the line as well. All together, the query returns any full line which does not begin with a tilde; that is, the lines we did not mark as looking like letters. We search for the pattern, and leave the replace blank; this will delete the lines that do not begin with a tilde.

**Do this**

search for: `\n[^~]+`

replace with: `\n`

7. To turn this text file into a csv suitable for network analysis, we'll want to separate it out into one column for Sender, one for Recipient, and one for Date, each separated by a single comma. Notice that most lines have extraneous page numbers attached to them; we can get rid of those with regular expressions. There's also usually a comma separating the month-date and the year, which we'll get rid of as well. In the end, the first line should go from looking like the following:

`~Sam Houston to J. Pinckney Henderson, December 31, 1836 51`

to looking like the following:

`Sam Houston, J. Pinckney Henderson, December 31 1836`

**Read through before you do anything.** You will start by removing the page number after the year and the comma between the year and the month-date. To do this, first locate the year on each line by using the regex:

  `[0-9]{4}`

We can find any digit between 0 and 9 by searching for `[0-9]`, and `{4}` will find four of them together. Now extend that search out by appending `.+` to the end of the query; as seen before, it will capture the entire rest of the line. The following query:

    `[0-9]{4}.+`

will return, for example, "1836 51", "1839 52", and "1839 53" from the first three lines of the text. We also want to capture the comma preceding the year, so add a comma and a space before the query, resulting in the following:

     `, [0-9]{4}.+`

which will return ", 1836 51", ", 1839 52", etc.

The next step is making the parenthetical groups which will be used to remove parts of the text with find-and-replace. In this case, we want to remove the comma and everything after 'year', but not the year or the space before it. Thus our query will look like the following:

    `(,)( [0-9]{4})(.+)`

with the comma as the first group `\1`, the space and the year as the second `\2`, and the rest of the line as the third `\3`. Given that all we care about retaining is the second group (we want to keep the year, but not the comma or the page number), what will the replace look like? You only want to keep the **second** group.

{{< expand "It'll look like this" >}}
search: `(,)( [0-9]{4})(.+)`
replace: `\2`

Now go ahead and do that.
{{< /expand >}}

8. Find the tildes that we used to mark off our text of interest, and replace them with nothing to delete them.

9. Finally, to separate the Sender and Receiver by a comma, we find all instances of the word "to" and replace it with a comma. Although we used `\b` and `\b` to denote the beginning and end of a word earlier in the lesson, we don't exactly do that here. We include the space preceding "to" in the regular expression, as well as the ` \b` to denote the word ending. Once we find instances of the word and the space preceding it,` to\b` we replace it with a comma `,`.

**Do this**
search: `(\b to \b)`
replace: `,`

10. You now have a document that contains senders, recipients, and the date in three columns separated by commas. Well done! You'll need to insert a line right at line 1 though to make this clear: at the top of the file, add a new line that simply reads "Sender, Recipient, Date".

11. You may notice that some lines still do not fit our criteria. Line 22, for example, reads "Abner S. Lipscomb, James Hamilton and A. T. Bumley, AugUHt 15, ". It has an incomplete date; we don't need to worry about these for our purposes.

More worrisome are lines, like 61 "Copy and summary of instructions United States Department of State, " which include none of the information we want. We can get rid of these lines later in a spreadsheet.

The only non-standard lines we need to worry about with regular expressions are the ones with more than 2 commas, like line 178, "A. J. Donelson, Secretary of State [Allen,. arf interim], December 10 1844". Notice that our second column, the name of the Recipient, has a comma inside of it. If you were to import this directly into a spreadsheet, you would get four columns, one for Sender, two for Recipient, and one for date, which would break any analysis you would then like to run. Unfortunately these lines need to be fixed by hand, but happily regular expressions make finding them easy. The following query:

`.+,.+,.+,`

will show you every line with more than 2 commas, because it finds any line that has any set of characters, then a comma, then any other set, then another comma, and so forth. You can just 'find all' and then Sublime will show you the lines to fix manually.

### Phew

That was a lot of work. Save your file with a new name: `correspondence.csv`. You now have a document that can be visualized as a network and analyzed as such.

But it's still pretty messy - it was, after all, OCR'd in the first place. We'll fix that in the [open refine](week/3/open-refine/) exercise.
