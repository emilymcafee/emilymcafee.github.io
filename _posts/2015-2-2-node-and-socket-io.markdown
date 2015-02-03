---
layout: post
title:  "Working with Node.js and Socket.io"
date:   2015-2-2 12:30:00
---

I have a lot to say about this topic, so I'm going to break this into three sections. The first deals with the thought process of creating a Node.js app for my last group project. The second and third will be more technical, as I troubleshoot two major problems with Socket.io and the Twit module.


-----

##Part I

As described in my [previous blog post](http://emilymcafee.github.io/2015/01/31/twitter-burst.html), my group last week built a real-time Twitter hashtag competition game. We planned on using the Twitter Streaming API to open a connection with Twitter and stream tweets onto the page for each round, but this proved way more difficult than we had initially thought. So I built a small Node app that handled the connection with Twitter and provided our app with a custom stream of real-time tweets.

**Why Node & why not Rails:**

Here is how I would have tried to solve this problem with Rails (disregarding Faye):

-	Create something on the backend that can handle the connection with Twitter
-	Using an AJAX request from the frontend, repeatedly (e.g. every second) contact that 'something' in our Rails app and ask if anything has changed with Twitter
-	Update the page accordingly if something has changed

There are a number of difficulties here, but the most important one has to do with the philosophy behind TwitterBurst: the game should be driven by people using Twitter in real time. When someone tweets, *that* is what makes the circle grow bigger or the ticker increment. The app is driven by the event of a tweet, not the event of an AJAX request.

This means that the app needs a persistent connection with Twitter so that it can just wait and listen for tweets to come through. It also needs a persistent connection with the browser so it can update the page immediately when a tweet comes through. Rails (well, Ruby) cannot do this. Ruby, by nature, is synchronous and blocking; only one process can happen at a time, and it cannot persist the kind of connection we need. 

Javascript, on the other hand, is asynchronous, non-blocking, and single-threaded. It can handle simultaneous processes without locking up the application. By its nature, it is the appropriate tool for the job. Up to this point, we have only used JS on the frontend of an application, but here we need it on the backend, to handle the business logic of securely connecting to Twitter, sending search parameters, and processing the returned data. 

Enter Node.

(Important side note: I realize it may sound like I'm talking about Rails and Node as if they are analogous, but they are not. Rails is a web framework, and Node is not. Node is more analogous to Rack middleware + Ruby interpreter for Javascript. Express sits on top of it as a web framework.)

Node enables us to use JS on the backend. It can handle the connection with Twitter as well as the connection with the frontend, and it can persist those connections. From here, we can move from a request/response model and into a publish/subscribe model with persistent connections (i.e. web sockets!!)

The last thing that I want to say about our decision to create a separate Node app is that it allowed us to abstract a very discrete element of our app into a separate tool. Once it was built and deployed, we could interface with this tool and employ at the right moment, but there was no need for it to be integrated with the rest of our application. In the future, we could even re-use this tool with another application, like a Twitter analytics app or something.


**Technologies used:**

I'm using [Express](http://expressjs.com/) as a lightweight framework for working with Node. On the server side, the [Twit module](https://github.com/ttezel/twit) is handling the connection with Twitter. To use web sockets (i.e. the persistent connections), I'm using [Socket.io](https://github.com/Automattic/socket.io).

**Problems: where do we stand?**

We presented TwitterBurst when the Node app had been up & running for about 2 days, and at that time, there were a few major glitches. They weren't immediately apparent when we demoed the app, but I could tell that something was off. Upon further investigation, I found problems galore! They mostly fall into two categories, described here:

**(1)** Tweets that were displayed in the browser *looked* like they were changing quickly, but when compared to what was coming through the console, often only a handful were getting through onto the screen. I logged each tweet to the console as a diagnostic tool, and it was very obvious that there was a *major* discrepancy between the actual tweets coming through and what was being displayed.

**(2)** The app would get bogged down when the game was played multiple times. With each round, the stream would get slower, and slower, and slower. *Moreover*, it oddly seemed like the results for later rounds all included key words from rounds prior. An example: first round was "snow" vs. "cold." Lots of results for both. Then I played a few more rounds, and the app slowed down with each one (this included playing "patriots" during the Superbowl -- zero results.) Here's the kicker. When I played "happy," the first two results (out of just 4 total) were "happy as can be bc snow day :)))" and "I'm so happy I'm stranded in Florida and not in three feet of snow." Snow was not a parameter of the current search, but it was of the original search ~5 games ago.

The next two sections of this post will explore these two problems. It's a little verbose, and mostly part of my own comprehension, but feel free to stick with me if you'd like :)

----

##Part II

###**PROBLEM:** Not all of the tweets are making it onto the page.

**EXPLANATION:** I was using an "OR" search (that is, combining both search keywords in one socket) instead of creating a separate socket for each keyword.
	
**SOLUTION:**
With both words coming through in one stream, additional processing was required on the client side to parse out the two words into their respective containers. My first solution to this was creating an if / else if conditional to search each tweet for a keyword and sort the tweet accordingly. Unfortunately, I think that this sorting was occuring synchronously and as a result, the vast majority of incoming tweets were blocked and never made it on the screen. I needed to make this processing asynchronous in order to handle the volume of tweets coming through in those 30 seconds. If I *had* to sort this information on the client side, I would next explore [this library](https://github.com/caolan/async) as a resource for handling the processing.

...but that would just be ignoring the root of the problem, which is that both searches are coming through in one stream. That brings me to the better solution.

**BETTER SOLUTION:** A socket for each search.

Let's start by making adjustments on the client side. Here's the original client-side of the app. Note the singular socket with an 'or' search and the additional processing with conditionals:

{% gist 7dc1888c80236e454a63 %}

I refactored to put the logic of creating a socket and appending the tweets to the page in a function:

{% gist 6dca7b82542efa471877 %}

And on the server side, I changed the search parameters to only accept one keyword, instead of two.

I was able to get the app to another valid state with this refactoring and the creation of two sockets. However. It still had the *exact* same problem as before -- the tweets came back in the same stream. How could it be that they were coming back together, especially since I was creating the sockets separately? 

That's when I found [this gold nugget.](https://github.com/Automattic/socket.io/wiki/How-do-I-send-a-response-to-all-clients-except-sender%3F)

Creating two web sockets was the correct solution, but both of those sockets were still emitting tweets to *all* of the sockets (including themselves!!). So we still had a single stream that would require client-side processing. Here's the Node app in this state (server side):

{% gist 25a07d9c5d5fd41c475d %}

Line 23 is to blame: `io.sockets.emit('stream', tweet.text);`

This sends the stream to ALL clients, including the socket that is *doing* the sending.

Next I tried this:
`socket.broadcast.emit('stream', tweet.text);`

This appeared to solve my problem at first, because the streams were separated by keyword without any processing! Funny enough, though, the results were showing up in the opposite container from where they should be. "snow" and "waterbottle" created two separate streams (yay!), but the results were populating under the wrong headline. This is because 'broadcasting' will send the stream to all clients, *except* for the socket that is doing the sending. Basically I had created a chatroom out of TwitterBurst, where the two keywords were sending streams of tweets to each other. This was actually a tempting solution at first, because on the page it looks like we only have two sockets at once, so why not just switch them on the page and call it a day? That's when you need to think about multiple people using the app at once...

Okay, so the streams need to be isolated from each other, and from all other sockets that may be in existence at a particular time. How to handle this?

`socket.emit('stream', tweet.text, { receivers: 'socket'})`

Boom. Instead of io.sockets, we are referring to the singular 'socket,' and actions pertaining to that particular socket. We are even specifying its *receiver*, which, in this case, is just itself. This is a game changer! Now, we have an isolated socket that uses one search parameter and gets back a stream of tweets on that keyword alone.

-----

##Part III

###**PROBLEM:** App stops functioning properly after first round.

**EXPLANATION:** The client stops listening after 30 seconds, but the server does not close the original stream with Twitter. Consequently, the original stream will continue being emitted for multiple games, even if the search terms have changed. Subsequent games will not create a fresh connection with Twitter--instead, subsequent games will just search within the original stream (hence "I'm so happy I'm stranded in Florida and not in three feet of snow.") 

**SOLUTION:** I believe this has to do with using the Twit module and closing/restarting the connection there. I'm hoping to implement a solution where I can also pass a # of seconds as a parameter for the timeout with the keyword to stream on. Check back soon for a solution...hopefully!



