---
layout: post
title:  "TwitterBurst!"
date:   2015-1-31 13:01:17
---

Week 9 of General Assembly WDI was a 4-day group project; my group built a head-to-head, real-time hashtag competition with the Twitter streaming API. The game runs for 30 seconds, and whichever word has the most tweets come through in those 30 seconds wins. The name "TwitterBurst" comes from a feature that has yet to be implemented: growing and shrinking circles represent the score in real time, and the circle for the winning hashtag will 'burst' at the conclusion of the 30 seconds.

**Visit the app:**

When [visiting TwitterBurst][burst], you may need to also awaken [the Node server][node] that is hosted for free on Heroku!


**My role in the project:**

-	Project lead, delegated tasks to teammates and oversaw workflow.
-	Designed database structure.
-	Implemented interfacing with db using AJAX to (1) automatically save game to database upon completion, (2) update leaderboard, and (3) display updated leaderboard and user stats without leaving the page.
-	Built a Node.js app to serve TwitterBurst with a stream of tweets based on custom parameters (depending on user and computer-generated input for each game).


**My fav features:**

-	The obvious winner is the Node app, but I'm going to save that for an upcoming blog post.
-	The circles. I can't take credit for the circle size algorithm, but I just love the thought process behind them. Not only do they give the user something to watch during those long 30 seconds, but they really represent the core idea of the app. It's not just how much a certain word is being used overall, but it's how much in *relation* to another word, at a particular moment in time. My inspiration for this representation was [Google Ngram Viewer][ngrams], one of my all-time favorite projects.
-	The power of the real-time web. Javascript is not just for animations; it's a crazy powerful tool that lets you do things like interact with your database without leaving the page. And way more that I don't even know about yet. We tried to make use of this by building a single-page app.


**What's next:**

-	Again, will leave Node app improvements to the next blog post.
-	Smoothing out circle animations, possibly allowing circles to grow beyond the size of the div they are currently in (they max out at 300px I believe).
-	Winning circle explosion and proper congratulations to the winner.
-	2 player mode.
- 2+ player mode?!?


**Learned:**

-	Asynchronous programming
-	Intro to web sockets
-	Managing group dynamics and work
-	Thoughtful database design is a challenge worth taking on


**Technologies used:**

-	Ruby on Rails
-	PostgreSQL
-	RSpec
-	jQuery / AJAX
-	Node.js / Express / Socket.io
-	Omniauth with Twitter
-	Twitter streaming API


[burst]: http://twitterburst.herokuapp.com/
[ngrams]: https://books.google.com/ngrams
[node]:	http://twitter-burst-node.herokuapp.com/