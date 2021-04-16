---
title: "Point Map with Google Maps"
description: "A simple map from a spreadsheet"
date: 2020-01-28T00:10:51+09:00
draft: false
author: "Shawn Graham"
---

The Google ecosystem has a wide variety of tools that can be made to work in concert. One thing that can be done is to use a Google sheet as the data source for a Google map. If you have data on your own machine in a spreadsheet file or a csv file, you can upload your data to Google Sheets and then plumb that data through the maps. Boom! A very useful process.

Jack Dougherty and Ilya Ilyankou have put together an walk through of the process for their book, '[Hands-on Data Visualization](https://handsondataviz.org/).' It's really a very excellent book that you should bookmark. Their Chapter 7 discusses a wide variety of ways to map your data.

For our purposes, I want you to
+ work through their section on '[Point Map with Google My Maps](https://handsondataviz.org/mymaps.html)'
  + their sample data is on [this spreadsheet](https://docs.google.com/spreadsheets/d/11nSoyGUxDJiBe5VZflDnaaqgWMdWyIUNnjNLMlGOPLk/edit#gid=312385679). The very first step of their instructions has you making a copy of this sheet, to power your own map.
+ Once you get to the end of their section, follow the instructions in step 18 to embed the map as an iframe. Copy the html code, and paste it directly into your `reflection.md` file. When you drop that file - **with the .md extension** into your github repo, Github should recognize that html and display your map.
+ **Change the source data** to be more Ottawa-specific. List museums, parks, and historic sites from across the region. Alternatively, create a map that displays all the places mentioned in a historical document or academic article.
