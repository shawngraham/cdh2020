---
title: "Mapping With Python and Folium"
description: "Webmaps, not GIS"
date: 2020-01-28T00:10:09+09:00
draft: false

---

### Geocoding and Mapping in Python

There is a complete [jupyter notebook](/data/mapping.ipynb) you can save to your own machine, and run by first starting Jupyter Notebooks; you'll want this [list of Ottawa places](/data/ottawa-places.csv).


_This notebook file is adapted from Melanie Walsh 2021 'Introduction to Cultural Analytics: Mapping' [https://melaniewalsh.github.io/Intro-Cultural-Analytics/Mapping/Mapping.html](https://melaniewalsh.github.io/Intro-Cultural-Analytics/Mapping/Mapping.html). You are encouraged to follow the remainder of her tutorial to learn how to add a custom map background, and how to publish your resulting map to the web._

**The code below reproduces that jupyter notebook and of course assumes you are working in a jupyter notebook.**

## Mapping with Python


In this lesson, we're going to learn how to analyze and visualize geographic data.


# Geocoding with GeoPy


First, we're going to geocode data — aka get coordinates from addresses or place names — with the Python package [GeoPy](https://geopy.readthedocs.io/en/stable/#). GeoPy makes it easier to use a range of third-party [geocoding API services](https://geopy.readthedocs.io/en/stable/#), such as Google, Bing, ArcGIS, and OpenStreetMap.

Though most of these services require an API key, Nominatim, which uses OpenStreetMap data, does not, which is why we're going to use it here.


### Install GeoPy

```python
!pip install geopy
```

### Import Nominatim


From GeoPy's list of possible geocoding services, we're going to import Nominatim:

```python
from geopy.geocoders import Nominatim
```

### Nominatim & OpenStreetMap


<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b0/Openstreetmap_logo.svg/256px-Openstreetmap_logo.svg.png" border=2 >


Nominatim (which means "name" in Latin) uses [OpenStreetMap data](https://www.openstreetmap.org/relation/174979) to match addresses with geopgraphic coordinates. Though we don't need an API key to use Nominatim, we do need to create a unique [application name](https://operations.osmfoundation.org/policies/nominatim/).


Here we're initializing Nominatim as a variable called `geolocator`. Change the application name below to your own application name:

```python
geolocator = Nominatim(user_agent="GIVE-A-NAME-HERE-app", timeout=2)
```

To geocode an address or location, we simply use the `.geocode()` function:

```python
location = geolocator.geocode("Wellington Street Ottawa")
```

```python
location
```

---

### An Alternative: Google Geocoding API


The Google Geocoding API is superior to Nominatim, but it requires an API key and more set up. To enable the Google Geocoding API and get an API key, see [Get Started with Google Maps Platform](https://developers.google.com/maps/gmp-get-started) and [Get Started with Geocoding API](https://developers.google.com/maps/documentation/geocoding/start).

**If you want to just continue without mucking about with Google, skip down to the heading 'Continue'**

```python tags=["hide-input"]
# once you get an API Key from Google, you would uncomment the three lines below,
# inserting the API key into the appropriate spot in the second line

#from geopy.geocoders import GoogleV3
#google_geolocator = GoogleV3(api_key="YOUR-API-KEY HERE")
#google_geolocator.geocode("Wellington Street")
```

### Get Address

```python
print(location.address)
```

### Get Latitude and Longitude

```python
print(location.latitude, location.longitude)
```

### Get "Importance" Score

```python
print(f"Importance: {location.raw['importance']}")
```

### Get Class and Type

```python
print(f"Class: {location.raw['class']} \nType: {location.raw['type']}")
```

---

## Continue...


### Get Multiple Possible Matches

```python tags=["output_scroll"]
possible_locations = geolocator.geocode("Wellington Street", exactly_one=False)

for location in possible_locations:
    print(location.address)
    print(location.latitude, location.longitude)
    print(f"Importance: {location.raw['importance']}")
```

```python
location = geolocator.geocode("Wellington Street, Ottawa Ontario")

print(location.address)
print(location.latitude, location.longitude)
print(f"Importance: {location.raw['importance']}")
```

## Geocode with Pandas


To geocode every location in a CSV file, we can use Pandas, make a Python function, and `.apply()` it to every row in the CSV file.

```python
# you might need to uncomment the next line and run it to install pandas
# pandas is a package that lets you work with tabular data etc

#!pip install pandas

import pandas as pd
pd.set_option("max_rows", 400)
pd.set_option("max_colwidth", 400)
```

Here we make a function with `geolocator.geocode()` and ask it to return the address, lat/lon, and importance score:

```python
def find_location(row):

    place = row['place']

    location = geolocator.geocode(place)

    if location != None:
        return location.address, location.latitude, location.longitude, location.raw['importance']
    else:
        return "Not Found", "Not Found", "Not Found", "Not Found"
```

To start exploring, let's read in a CSV file with a list of places in and around Ithaca.

```python
ottawa_df = pd.read_csv("ottawa-places.csv")
```

```python
ottawa_df
```

Now let's `.apply()` our function to this Pandas dataframe and see what results Nominatim's geocoding service spits out.

```python
ottawa_df[['address', 'lat', 'lon', 'importance']] = ottawa_df.apply(find_location, axis="columns", result_type="expand")
ottawa_df
```

# Making Interactive Maps


To map our geocoded coordinates, we're going to use the Python library [Folium](https://python-visualization.github.io/folium/). Folium is built on top of the popular JavaScript library [Leaflet](https://leafletjs.com/).


To install and import Folium, run the cells below:

```python tags=["output_scroll"]
!pip install folium
```

```python
import folium
```

### Base Map


First, we need to establish a base map. This is where we'll map our geocoded Ithaca locations. To do so, we're going to call `folium.Map()`and enter the general latitude/longitude coordinates of the Ithaca area at a particular zoom.

(To find latitude/longitude coordintes for a particular location, you can use Google Maps, [as described here](https://support.google.com/maps/answer/18539?co=GENIE.Platform%3DDesktop&hl=en).)

```python
ottawa_map = folium.Map(location=[45.42, -75.69], zoom_start=12)
ottawa_map
```

### Add a Marker


Adding a marker to a map is easy with Folium! We'll simply call `folium.Marker()` at a particular lat/lon, enter some text to display when the marker is clicked on, and then add it to our base map.

```python
folium.Marker(location=[45.385858, -75.695004], popup="Crafting Digital History!").add_to(ottawa_map)
ottawa_map
```

### Add Markers From Pandas Data


To add markers for every location in our Pandas dataframe, we can make a Python function and `.apply()` it to every row in the dataframe.

```python
def create_map_markers(row, map_name):
    folium.Marker(location=[row['lat'], row['lon']], popup=row['place']).add_to(map_name)
```

Before we apply this function to our dataframe, we're going to drop any locations that were "Not Found" (which would cause `folium.Marker()` to return an error).

```python
found_ottawa_locations = ottawa_df[ottawa_df['address'] != "Not Found"]
```

```python
found_ottawa_locations.apply(create_map_markers, map_name=ottawa_map, axis='columns')
ottawa_map
```

### Save Map

```python
ottawa_map.save("Ottawa-map.html")
```

## Torn Apart / Separados


The data in this section was drawn from [Torn Apart / Separados Project](https://github.com/xpmethod/torn-apart-open-data). It maps the locations of Immigration and Customs Enforcement (ICE) detention facilities, as featured in [Volume 1](http://xpmethod.plaintext.in/torn-apart/volume/1/).


Go to [https://github.com/melaniewalsh/Intro-Cultural-Analytics/tree/master/book/data](https://github.com/melaniewalsh/Intro-Cultural-Analytics/tree/master/book/data) to get the data files for this next section OR insert the following string pattern into the file names as appropriate to load directly from the web:

eg, where the code says `ICE_df = pd.read_csv("../data/ICE-facilities.csv")`

you'd change that to

`ICE_df = pd.read_csv("https://raw.githubusercontent.com/melaniewalsh/Intro-Cultural-Analytics/master/book/data/ICE-facilities.csv`)`


### Add a Circle Marker


There are a few [different kinds of markers](https://python-visualization.github.io/folium/quickstart.html#Markers) that we can add to a Folium map, including circles. To make a circle, we can call `folium.CircleMarker()` with a particular radius and the option to fill in the circle. You can explore more customization options in the [Folium documentation](https://python-visualization.github.io/folium/modules.html#folium.vector_layers.CircleMarker). We're also going to add a hover `tooltip` in addition to a `popup`.

```python
def create_ICE_map_markers(row, map_name):

    folium.CircleMarker(location=[row['lat'], row['lon']], raidus=100, fill=True,
                popup=folium.Popup(f"{row['Name'].title()} <br> {row['City'].title()}, {row['State']}", max_width=200),
                  tooltip=f"{row['Name'].title()} <br> {row['City'].title()}, {row['State']}"
                 ).add_to(map_name)
```

```python tags=["output_scroll"]
ICE_df = pd.read_csv("https://raw.githubusercontent.com/melaniewalsh/Intro-Cultural-Analytics/master/book/data/ICE-facilities.csv")
ICE_df
```

```python
US_map = folium.Map(location=[42, -102], zoom_start=4)
US_map
```

```python
ICE_df = ICE_df.dropna(subset=['lat', 'lon'])
```

```python
ICE_df.apply(create_ICE_map_markers, map_name=US_map, axis="columns")
US_map
```

## Choropleth Maps


```margin Choropleth Map
Choropleth map = a map where areas are shaded according to a value
```


The data in this section was drawn from [Torn Apart / Separados Project](https://github.com/xpmethod/torn-apart-open-data). This data maps the "cumulative ICE awards since 2014 to contractors by congressional district," as featured in [Volume 2](http://xpmethod.plaintext.in/torn-apart/volume/2/).


To create a chropleth map with Folium, we need to pair a "geo.json" file (which indicates which parts of the map to shade) with a CSV file (which includes the variable that we want to shade by).


The following data was drawn from [the Torn Apart / Separados project](https://github.com/xpmethod/torn-apart/tree/master/data/districts)

```python
US_districts_geo_json = "../data/ICE_money_districts.geo.json"
```

```python
US_districts_csv = pd.read_csv("../data/ICE_money_districts.csv")
```

```python
US_districts_csv = US_districts_csv .dropna(subset=['districtName', 'representative'])
```

```python
US_districts_csv
```

```python
US_map = folium.Map(location=[42, -102], zoom_start=4)

folium.Choropleth(
    geo_data = US_districts_geo_json,
    name = 'choropleth',
    data = US_districts_csv,
    columns = ['districtName', 'total_awards'],
    key_on = 'feature.properties.districtName',
    fill_color = 'GnBu',
    line_opacity = 0.2,
    legend_name= 'Total ICE Money Received'
).add_to(US_map)

US_map
```

### Add a Tooltip to Choropleth

```python
tooltip = folium.features.GeoJson(
    US_districts_geo_json,
    tooltip=folium.features.GeoJsonTooltip(fields=['representative', 'state', 'party', 'total_value'], localize=True)
                                )
US_map.add_child(tooltip)
US_map
```
