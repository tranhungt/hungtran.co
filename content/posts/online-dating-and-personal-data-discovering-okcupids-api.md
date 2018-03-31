---
title: "Online Dating and Personal Data Discovering Okcupids Api"
date: 2014-08-29T19:01:12-05:00
draft: false
---

People seem to be more and more concerned about online privacy, but somehow seem to be willing and apt to give out their personal information more than ever, especially when it comes to online dating.

### A New Generation of Disclosure
A few weeks ago, I wrote a piece on [discovering Tinder's private API](http://hungtran.co/discovering-tinders-private-api/) and wrote the [tinder nodejs module](http://hungtran.co/wrapping-up-tinders-private-api-into-a-nodejs-module/). Although I did not mention any serious exposure of private data, there was a time when Tinder would reveal exact latitude/longitude data and exact birthdates of a match candidate.

OKCupid isn't so innocent in such domain either. Since OKCupid is appreciated in the way it allows users to understand each other on a deeper and wider array of attributes - likes, dislikes, and personality personalities - it stores a massive amount of data regarding its users. All this data is then accessible to anyone with a valid account. Information such as height, birthday, lat-long coordinates and even income levels are readily available.

In a time when more people are relying on the internet to find dates, it's reasonable to expect that people are more willing to provide as much information as possible to better open up themselves to match matching. The problem is that few would think that this information lives on the internet and becomes open to the general public.


Is there a better way to present personal information to potential lovers without exposing oneself too much?

### OKCupid's API
In general, any type of online service is susceptible to sniffing, scraping, and scrutinity if one looks hard enough. After playing around with Chrome's inspect tool, I was able to find a series of endpoints that exposes OKCupid's json API.

I also took the liberty to write a Nodejs wrapper for it. For those who are inclined:

https://github.com/tranhungt/okcupidjs

These API endpoints will allow a user to access the quickmatch interface, get the username, rate the person, get a nice json output of the user's entire profile, and a few more.

As a previous user of OKCupid, I found it frustrating to spend countless hours rating users and messaging them with little response. SO this API wrapper will automate some of the work and give you more time to connect with people who are more interested in you.


---
Links:

NodeJS OKCupid API Wrapper: 
https://github.com/tranhungt/okcupidjs

Charles Proxy Tutorial on discovering private API's:
http://hungtran.co/discovering-tinders-private-api/
