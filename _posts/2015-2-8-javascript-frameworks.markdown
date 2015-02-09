---
layout: post
title:  "Javascript frameworks"
date:   2015-2-8 11:00:00
---

This week at GA we split in two, and my group was introduced to the concept of frontend JS frameworks, specifically Backbone. Others have already drilled into the nitty-gritty of Backbone [(hey Andy)](http://andrewsunglaekim.github.io/What-Is-this-Backbone-dont-hurt-me/), so I'm going to take a step back and think about this at a high level.

For me, learning about JS frameworks after 9 weeks of of RoR was a little... ground shaking. We built an app called Grumblr that was basically just a pseudonym for the well-known [Todo app](http://todomvc.com/). And it did (almost) everything that we had previously used Rails for. It had class-like structure for handling data. It had complete CRUD functionality. It had show views, index views, etc. It had a router that could direct the user around the page with real URLs, and pass parameters with those URLs. And it did all of this without ever reloading the page or navigating away from the initial load. We did incorporate Rails, which now served a singular purpose as a persistent data store. The Rails piece hardly even served HTML; obscured behind "/api/" endpoints, it mostly responded with JSON to facilitate data exchange for our Backbone layer.

Hmm.

I'm going to disregard the angst/anxiety that sometimes comes with big changes, and instead just explore what Javascript frameworks are all about.


### **Overview**

At their root, I suppose these frameworks/libraries are about organizing your Javascript (avoiding jQuery soup). This structure is typically achieved by separating observable data (Models) from the presentation of that data to the user (View).

Where things get tricky is in how those two things get linked up again. This "binding" is key; when something changes with the model, the view should update, and when something changes on the view, the model should update accordingly. The piece that does the binding varies: it can be a controller (MVC), a presenter (MVP), a view model (MVVM), or *whatever* (MV*). That data binding is pretty much the definition of the real-time web, btw. 

So, using an organizational framework (like Backbone), we are creating a mirror of our server-side application on the client side, and then maintaining a continuous connection between the two.


### **What it boils down to: listening**

What we are talking about here is everything that happens *after* an initial page load. On a single-page app, there should be 0 refreshing after that point -- no reloading the page. Any time the data changes after that initial load (whether through user interaction or new data coming from the server), the UI should automatically sync up to that data. 

Instead of the client making requests and the server returning responses, the client is *subscribing* to changes in data and the server is continually *publishing* whatever changes may come. This is a dynamic, flexible, asynchronous way to handle changes in data, and what makes it all possible is that Javascript can listen. Listen to user interaction, listen to changes in data on the server, etc -- Javascript can listen to those events, and respond. (A framework like Backbone enhances that listening and automates responses.)


### **Backbone.js key concepts**
-	Provide structure to your JS
-	Handle data with models and collections, and present it with views
-	Keep your "truth" (data) out of the DOM
-	Recreate your application layer on the client side, so that whenever your data changes, view will update automatically
-	Minimal -- not opinionated, allows you to decide on what organization is best for you
-	Data binding between views and models/collections


### **Where to next? React**

[React](http://facebook.github.io/react/index.html) (by Facebook) is a library for the UI, described as the V in MVC. I have a rudimentary understanding of how React works, and I'm totally fascinated by it -- specifically the "virtual DOM." React doesn't want to deal with mutating the DOM piece-by-piece. Instead, it mimics the behavior of a server-rendered app by re-rendering the entire thing when there's an update. Obviously this would be ridiculously inefficient if you *actually* re-rendered everything, which is why React uses a virtual DOM. For every update, React is tracking the differences between separate representations of the DOM. It reconciles those differences with the most minimal set of DOM mutations possible, and then executes the updates. That's my understanding, at least :)
