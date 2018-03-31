---
title: "Wrapping Up Tinders Private Api Into a Nodejs Module"
date: 2014-08-05T19:00:18-05:00
draft: false
---

After [discovering Tinder's private API's](http://hungtran.co/discovering-tinders-private-api/), I wanted to document everything I found. What better way than to write a nodejs module that would wrap those API calls into something simple and easy to use?

The nodejs module can be found here:
https://github.com/tranhungt/tinder_pro

The intent was to make something modular that would be very open to interpretation, by emulating the tinder API as close as possible.

At the base level, there's a Requester object that handles all interactions between the node server and Tinder's API. It would handle all http requests and necessary headers to emulate  an authentic request. It would take in the necessary url routing, the data to be sent, and give back the response it gets from Tinder.

Then there's a Client object that intermediates between the requester object and the user. It abstracts the API into simple functions such as getting nearby users or liking a user.

Building this module, I was able to practice modular code building, by breaking up types of services into different objects and submodules and delegating specific tasks to each function. It also made me very aware of the http requests and specific headers that are needed to emulate a private API. 

It was also my first time publishing to NPM, which was quite easy to do. It's fascinating to be able to write a piece of code and release to the public for reuse. It makes you think more about your own coding practices and whether it is something that could be easily understood and forked by someone else.

Code on Github:
https://github.com/tranhungt/tinder_pro

Link to NPM repo:
https://www.npmjs.org/package/tinder_pro
