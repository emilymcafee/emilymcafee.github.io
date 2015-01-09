---
layout: post
title:  "What the heck is JSON?"
date:   2015-1-7 18:00:00
---

I have fond memories of making my first request to an API, a mere three weeks ago. It was a request for WMATA Metro arrival data. Here's what the response looked like (for Columbia Heights):

{% highlight text %}
{"Trains":[{"Car":"6","Destination":"Grnbelt","DestinationCode":"E10","DestinationName":"Greenbelt","Group":"1","Line":"GR","LocationCode":"E04","LocationName":"Columbia Heights","Min":"BRD"},{"Car":"6","Destination":"Grnbelt","DestinationCode":"E10","DestinationName":"Greenbelt","Group":"1","Line":"YL","LocationCode":"E04","LocationName":"Columbia Heights","Min":"3"},{"Car":"-","Destination":"Train","DestinationCode":null,"DestinationName":"Train","Group":"2","Line":"--","LocationCode":"E04","LocationName":"Columbia Heights","Min":"8"},{"Car":"6","Destination":"Grnbelt","DestinationCode":"E10","DestinationName":"Greenbelt","Group":"1","Line":"GR","LocationCode":"E04","LocationName":"Columbia Heights","Min":"11"}]}
{% endhighlight %}

When this first came through, I freaked out because holy sh*t, I was getting real-time train arrival info from my command line and that's AWESOME. I could see the data! Right there! But... it was a string. Or something. In any case, I couldn't use Ruby to extract data in any meaningful way just yet. Surprise, surprise: it was JSON (JavaScript Object Notation). With a quick `JSON.parse` in Ruby, I had convenient access to every nook and cranny of that hunk of data.


**Some things to know about JSON:**

1. JSON is a **format for data transmisison** that is used for transferring data between a server and a web application. Basically, it's a lightweight notation that is used to move data from place to place.

2. It is **easy for humans** to read/write and **easy for machines** to parse/generate. 

3. Although JavaScript is in the name, and it was originally derived from JavaScript, JSON is **independent of any singular programming language**. This is perfect for exchanging information between a variety of locations.

4. JSON is constructed of two major data types: in Ruby we know them as **arrays** (ordered lists) and **hashes** (unordered key-value pairs).


Here is another example, a piece of a response from the Google Directions API, for a Metro trip between Columbia Heights and Navy Yard. Perhaps the JSON is more easily decipherable with whitespace and syntax highlighting:

{% highlight ruby %}
 {
   "routes" : [
      {
         "copyrights" : "Map data ©2015 Google",
         "legs" : [
            {
               "arrival_time" : {
                  "text" : "10:22pm",
                  "time_zone" : "America/New_York",
                  "value" : 1420773720
               },
               "departure_time" : {
                  "text" : "10:08pm",
                  "time_zone" : "America/New_York",
                  "value" : 1420772880
               },
               "distance" : {
                  "text" : "4.6 mi",
                  "value" : 7400
               },
               "duration" : {
                  "text" : "14 mins",
                  "value" : 840
               },
               "end_address" : "Navy Yard Metro Station, Washington, DC 20003, USA",
               "end_location" : {
                  "lat" : 38.876517,
                  "lng" : -77.00474899999999
               },
               "start_address" : "Columbia Heights Metro Station, Washington, DC 20010, USA",
               "start_location" : {
                  "lat" : 38.928711,
                  "lng" : -77.032454
               },
               "steps" : [
                  {
                     "distance" : {
                        "text" : "4.6 mi",
                        "value" : 7400
                     },
                     "duration" : {
                        "text" : "14 mins",
                        "value" : 840
                     },
                     "end_location" : {
                        "lat" : 38.876517,
                        "lng" : -77.00474899999999
                     },
                     "html_instructions" : "Subway towards Branch Ave",

#truncated for brevity
{% endhighlight %}   

You can imagine how straightforward it would be to drill into this piece of data using Ruby (or another language) and pull out precisely the information you are looking for. Pretty incredible.

**Topics of exploration:**

1.	How does JSON hold on to native data types? Answers [here][json].


2.	How does a programming language such as Ruby parse JSON? More [answers][docs]!

**Learned:**

JSON. Is. Amazing. This is my first time getting up close and personal with data interchange, and I feel like I just cracked open the world.

[json]:      http://www.json.org
[docs]:			http://www.ruby-doc.org/stdlib-2.0/libdoc/json/rdoc/JSON.html
