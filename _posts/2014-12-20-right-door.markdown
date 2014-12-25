---
layout: post
title:  "RightDoor"
date:   2014-12-20 13:01:17
---

Week 5 of General Assembly WDI was project week - a full 4 days for students to build individual projects. I set out to build a tool for the Washington Metro that would tell a user the best location to board the train in order to be dropped closest to their escalator at the transfer/exit. It also provides real-time train arrival info for your starting station and transfer station! Project week came before we got to Rails, so the app is built in Ruby using Sinatra. Over the holidays I'll be working on learning Rails and re-building RightDoor in that framework. 

**[Visit RightDoor](http://rightdoor.herokuapp.com)**

**[Visit the code on GitHub](https://github.com/emilymcafee/rightdoor)**

A good route to demo: Columbia Heights to Eastern Market.

**Musings:**

-	Wow. There's really no way for me to automate the calculation of the "right door" without manually scraping data for the location of each escalator in every station. I'm still committed to this idea, but in the meantime I'm implementing commenting on individual routes so that users can share their tips. This makes the app more of a knowledge-sharing platform for "hacking" your commute. 
-	How to get starting/ending station input? I struggled with this for awhile. Drop-down list? Check box? Input the line color and *then* the station? My instructor Jesse reminded me to think about my users here. Would it be tourists unfamiliar with the system, who need a map to look up their station? No -- this is for seasoned commuters, who only need to type in the station name and don't want to be bothered with much else. Hence the autofill.


**Learned:**

-	A tiny bit of jQuery (for auto-filling the stations). Still have a bug in this and unsure of how to fix.
-	How to use an API, sort of. I surprised myself with how quickly I was able to integrate WMATA's API, given we haven't talked about APIs in class yet. I'm now working on integrating with Google Maps (Directions) in order to outsource the calculation of routes and transfers. This all felt very foreign at first, until I realized that I'm basically just making a request for a big hash of data. 
-	Rails! I'm using this app as I embark on learning Rails; in a few weeks it will be re-launched on Rails.


**Technologies Used:**

-	Ruby
-	Sinatra
-	PostgreSQL
-	jQuery
-	[WMATA API](https://developer.wmata.com/docs/services/)
-	[Google Directions API](https://developers.google.com/maps/documentation/directions/)