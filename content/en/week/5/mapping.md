---
title: "Mapping"
description: "Webmaps, not GIS"
date: 2020-01-28T00:10:09+09:00
draft: false
weight: -3
---

### Telling Stories with StoryMaps

The historical stories we want to tell take place within and across space. The [Storymapjs](https://storymap.knightlab.com/) platform from the Knight Lab at Northwestern University was developed to make this easier to accomplish. Below is an example the Knight Lab put together concerning Arya Stark's journeys. Note how each point in the map can be annotated with videos, images, or other kinds of rich content. Each point on the map is annotated with text and media; the 'next arrow' moves us to the next point, implying a historical or temporal connection. Embedding this map into a text file required a single line of html,

```
<iframe src="https://storymap.knightlab.com/examples/aryas-journey/" width = 100% height = "400"></iframe>
```

<iframe src="https://storymap.knightlab.com/examples/aryas-journey/" width = 100% height = "400"></iframe>

Making a storymap requires using a Google account to log in. It uses the metaphor of 'slides' to describe each point on your map, and the interface itself is rather similar to what you might be familiar with for Powerpoint or Keynote.

Knightlab makes the following suggestions:

+ Keep it short. We recommend not having more than 20 slides for a reader to click through.
+ Pick stories that have a strong location narrative. It does not work well for stories that need to jump around in the map.
+ Write each event as a part of a larger narrative.
+ Include events that build up to major occurrences — not just the major events.

1. Login into [Storymap](https://storymap.knightlab.com/select/)
2. The editor will look roughly like this

![new storymap](/images/storymap/newstorymap.png)

3. Set your base map by clicking on options; there is a list of common base maps under 'options - Map Type'

![map types](/images/storymap/storymap2.png)

4. Add some basic info to your home slide (it'll be the red one, at the left, and you don't have any other slides yet). Each subsequent slide uses the same interface.

![basic](/images/storymap/storymap1.png)

5. Once you've got some slides - 5 would be fine - save the map, and then hit the 'share' button at right.

![share](/images/storymap/storymap3.png)

This will give you a link to share with folks; or if you scroll a bit further down, some html code which you can save in your notes.md or in a blog post or some other online document.

If you make changes subsequent to sharing, a new button appears beside 'save' called 'publish changes'.

### Webmapping with Leaflet

For this next section, I am going to copy and modify an exercise from the [Open Digital Archaeology Textbook Environment](https://o-date.github.io/draft/book/what-is-web-mapping.html) by Graham et al 2019; this particular section was contributed by [Neha Gupta of UBC](https://dngupta.github.io/).

First of all, a webmap is not a Geographic Information System or GIS. A webmap may be _derived_ from a GIS, but the key difference is that a GIS allows analytical questions to be asked of, and derived from, different _layers_ of spatial or spatialized data. Webmaps are used more for conveying the results of such analyses.

Tiled map service and [Web Map Service](http://www.opengeospatial.org/standards/wms) are two forms of Web-based mapping. A WMS is an interface that enables us (the clients) to request specific maps i.e. visual representations of geographic information from a geospatial database. The WMS server is called via a Universal Resource Link (URL) on an Internet-enabled desktop GIS. A request typically consists of the geographic layer (e.g. theme) and geographic area of interest. The response to a request results in geo-registered map images that are displayed and queried within a browser. Because the map is dynamically drawn upon request, and because the server typically uses the most current information from several layers in the geospatial database, WMS maps tend to load slowly.

Tile map services such as [TileMill Project](https://tilemill-project.github.io/tilemill/docs/crashcourse/introduction/), [OpenStreetMap](https://www.openstreetmap.org/), and [Stamen](http://maps.stamen.com/) all use one or more vector layers that have been rasterized into an image. This rasterized image is divided into 256 x 256 adjacent pixel images or ‘tiles’. This is usually the base layer in a web map.

Each tile is an image on the Web, which means that you can link to it. For example, the following URL points to a specific tile on the Web:

```
https://tile.openstreetmap.org/7/63/42.png
```

The three elements in the URL are:

1. tile.openstreetmap.org, the tile server name;
2. 7, the zoom level or z value of the tile; and finally
3. 63/42, the x and y values in the grid where the tile lives

The z value has a range between 0 and 21, where 21 returns a tile with greatest detail (and smallest sized tile).

![](https://o-date.github.io/draft/book/images/tile-pyramid.jpg)

'Image Tile Pyramid, [https://www.azavea.com/blog/2018/08/06/generating-pyramided-tiles-from-a-geotiff-using-geotrellis/tilepyramid/](https://www.azavea.com/blog/2018/08/06/generating-pyramided-tiles-from-a-geotiff-using-geotrellis/tilepyramid/)’

Once generated, the set of tiles are stored on disk, ready to be distributed rapidly to large numbers of simultaneous requests. Tiled maps load quickly precisely because they are pre-generated. They shift attention to map aesthetics and smooth map navigation, trading in functionality such as layer order, map scale and projection. [Alex Urquhart](http://alexurquhart.github.io/free-tiles/) maintains a list of tile services.

Data layers are typically added on top of the base layer. Data layers can be points, lines and polygons. These data layers are saved as [GeoJSON](https://geojson.org/), a format designed for representing on the Web, geographic features with their non-spatial attributes.

[Leaflet](http://leafletjs.com/) is a JavaScript library developed by [Vladimir Agafonkin](https://vimeo.com/106112939) for use with tiled maps. Launched in 2008, Leaflet has become widely used in tile web mapping because the library’s low-barrier customization and interactivity with map elements, and because of its simplified setup when compared to a WMS served map. Moreover, Leaflet’s compatibility with other Web 2.0 technologies and code-sharing platforms such as GitHub has encouraged an active community of ‘makers’.

1. Create a new directory on your machine and call it 'web-map'
2. Open a terminal or command prompt in that directory.
3. We're going to build a map now; in sublime text make a new file and save it as `index.html` in the `web-map` folder.
4. We'll begin by putting some important information about the building blocks we'll use in the 'header' of our file, between the `<head>` and `<\head>` tags. Copy and paste the following into your index.html:

```
<head>

	<title>A Demo WebMap</title>

	<meta charset="utf-8" />
	<meta name="viewport" content="width=device-width, initial-scale=1.0">

    <link rel="stylesheet" href="leaflet.css">
    <script src="https://cdn.leafletjs.com/leaflet-0.7.3/leaflet.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
    <script type="text/javascript"></script>
    </script>

</head>
```
This is telling the browser the name of this page, and it's telling the browser where to get the stylesheet and the code for making the map work. You don't have the `leaflet.css` or the `leaflet.js` files yet. Right-click, and 'save as' copies of these two files into your web-map folder from here: [leaflet.css](/data/leaflet.css) and [leaflet.js](/data/leaflet.js)

5. The next bit of code defines the base map:

```
<body>
<div id="mapid" style="width: 600px; height: 400px;"></div>
<script type="text/javascript">
var myMap = L.map('mapid').setView([47.5668, -52.7120],13);
L.tileLayer('https://stamen-tiles-{s}.a.ssl.fastly.net/toner/{z}/{x}/{y}.png',
	{maxZoom: 19
}).addTo(myMap);

```

We're creating a box on the page called 'mapid' and setting it's width and height in pixels. Then our script creates a thing called 'mymap' and defines some particulars about it: the center of the map (here, St. John's, Newfoundland), the zoom level, and the basemap layer. The final bit, `addTo(mymap);` does the work of putting all of that information on the map (using leaflet.js).

6. Now we're going to need some data for our map. Our point data will be encoded in a format called 'geojson', which is a kind of list. If you have geographic data in a csv table, with latitude and longitude columns, you could use [this tool](http://www.convertcsv.com/csv-to-geojson.htm) to convert it to geojson. For now, our geojson has two points in it:

```
{
   "type": "FeatureCollection",
   "features": [
  {
    "type": "Feature",
    "geometry": {
       "type": "Point",
       "coordinates":  [-52.7120, 47.5668]
    },
    "properties": {
    "Label":"Centre of our map!"
    }
  },
  {
    "type": "Feature",
    "geometry": {
       "type": "Point",
       "coordinates":  [ -52.720, 47.5675 ]
    },
    "properties": {
    "Label":"Not the centre of the map "
    }
  }
]
}

```

Copy that into a new file and save in your web-map folder as `point-data.geojson`

7. Now append the following to the `index.html`, which will tell leaflet where the data is and how to add it to our map:

```
// load a GeoJSON from external file
$.getJSON("point-data.geojson", function(data) {
  // add GeoJSON layer to the map once the file is loaded
  var datalayer = L.geoJson(data ,{
  onEachFeature: function(feature, featureLayer) {
  featureLayer.bindPopup(feature.properties.Label);
  }
}).addTo(myMap);
});

</script>

</body>
</html>
```

Save!

8. At the command prompt, you'll use python to start a webserver; the webserver acts like a regular server and makes these two files understandable by your web-browser: `$ python -m http.server`
9. The terminal will tell you now that your server is running at an address that will look something like this: `(http://0.0.0.0:8000/)`. Copy that full address and paste it into your browser's address bar.

Congratulations! You have made a webmap! Do you see how you could recenter the map on Ottawa? Add some more data? Change the base map to another style?

(You can upload your web-map folder to Github by dragging the folder onto a repository.)
