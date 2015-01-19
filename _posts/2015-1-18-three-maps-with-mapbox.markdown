---
layout: post
title:  "Three maps with Mapbox"
date:   2015-1-18 17:00:00
---

I spent the day experimenting with [Mapbox Studio][mapboxstudio] for the first time, and put together three maps using data from WMATA. 

**The first is a simple metro map:**

![Metro map]({{ site.url }}/assets/metro_map.jpeg)

![Metro map zoomed out]({{ site.url }}/assets/metro_zoom.jpg)


I pulled lat/long of each station [here][stationlist]. I had to do the connections by hand, though I'm sure there's a better way to do that. Thoughts on this map:

- How very different the actual geography of the metro is from the [WMATA system map][wmatamap], and how this influences/limits understanding of the region (by residents and tourists alike). Looking at this map (especially at higher zooms), the movement of trains through stations all of a sudden becomes tangibly connected to the movement of real people who live or work in various parts of the city/region. 
-	The story of putting public transportation on large-scale maps... I'm used to seeing regional or national maps with streets, highways, and interstates, but what about regional/national maps with rail and other forms of public transpo? It's a very different narrative.

**The second is a snapshot of WMATA's buses:**

![Bus snapshot]({{ site.url }}/assets/bus_snapshot.jpeg)

-	I pulled from [WMATA's bus position API][busposition] a snapshot of bus positioning around the city. I displayed it using the "Deviation" metric--that is, how far off-schedule a given bus is. The bigger the marker, the bigger the delay.
-	This map is pretty but it doesn't tell me much. I think I'm disappointed with it because it's not really loyal to the data it portrays. That is to say -- buses are not static. Bus *stops* are, and bus *routes* are, but the buses themselves are not. I think a good next step will be mapping the bus routes across the city.

**The third follows one 54 bus for 1 hour:**

![Bus 6500]({{ site.url }}/assets/bus_6500.jpeg)

-	Here's a small chunk of a bus traveling south along 14th Street NW. Mapping across time seemed more appropriate to the nature of a bus, so I wrote a short Ruby program to request the bus position of this particular bus (ID #6500) every 30 seconds over the course of an hour. I kept the Deviation metric in place (visible in red) -- here you can actually see the bus recovering from a delay -- note the shrinking size and return of yellow color as the bus travels south.
-	There's gotta be a better way... would love to portray this with animation or something more appropriate to mapping the passage of time and physical movement.

Well, that's it for dipping my toes today. I've really only scratched the surface of what Mapbox offers, so definitely will be digging into this platform more in the near future. We learned Javascript last week so the timing is right!

**Where to go from here:**

-	Using Mapbox.js
-	Continuous / dynamic integration of data
-	Animation of change over time, or physical movement

**Technologies used:**

-	Ruby
-	Javascript / fiddling with Mapbox.js
-	[CartoCSS](https://github.com/mapbox/carto/blob/master/docs/latest.md)
-	JSON, learning about GeoJSON

[mapboxstudio]: https://www.mapbox.com/mapbox-studio/#darwin

[stationlist]: https://developer.wmata.com/docs/services/5476364f031f590f38092507/operations/5476364f031f5909e4fe3311

[wmatamap]: http://www.wmata.com/rail/docs/color_map_silverline.pdf

[busposition]: https://developer.wmata.com/docs/services/54763629281d83086473f231/operations/5476362a281d830c946a3d68
