---
title: "What I See a Chrome Extension"
date: 2016-03-06T19:03:49-05:00
draft: false
---

Sometimes, you read an awesome article online and want to share it with you friend, so you send her the URL of the page, only to get the response "It's behind a paywall".

Or maybe you're logged into some web application and you want to show your friend what the page looks like and let her scroll through as if it was the real thing. It could be the case that you want to show your friend all the items in your Amazon cart before you make the purchase.

Perhaps you'd like to take a screenshot of a page and share it with your friend, but the page is too large and you'd have to keep scrolling and take multiple screenshots and somehow send it over in the right order for it to make sense.

Really, all you wanted to do was show your friend the page exactly as you see it. Not compressed in a Jpeg or chopped up into multiple screenshots, but an actually live web page as you are seeing it.

## How It Works
So I created a [Chrome Extension called "What I See"](https://chrome.google.com/webstore/detail/what-i-see/ghcflgkkdehjaohkbnaeghpjboibdpkn), which with a click of button, you can clone the web page exactly as it appears, and hosts it on a separate domain.

You are then given the URL of the new page, which is automatically copied onto your clipboard, and you can share it with your friend.

## Technology, and Some Things I Learned
In making this project, the first thing I learned is how to develop on the Chrome Extension platform. For security measures, Javascript is sandboxed between the Chrome Extension and the webpage itself. The only point of contact the two sides share is the HTML DOM. Luckily for me, I don't need to interact with the document's javascript. I do need the scripts that are being used on the page as it affects page rendering, and fortunately, all of that is included inside the document html upon page load.

Amazon S3 can be used as a static website host. That is, instead of having a web server serve up html content from a database, I can save all HTML documents in S3 and have all traffic pointing directly to the s3 bucket for retrieval.
This has a few advatanges, including not having to build or maintain a web server and a database.
It also eliminates any scalability issues on my side as Amazon's infrastructure now handles all the web requests.

This service also acts as a mini URL shortener. It is sensible that a long URL from the page being cloned should be reduced into something shorter and more manageable. That is easily done by hashing the url to a shortened code to basically guarantee uniqueness. However, if the user was to clone the exact website again, the hashing algorithm would return the same shortened code. A way to yield two different shortened codes from the same URL would be to introduce a time stamp to the URL string before the hashing algorithm is called.

## Where Can I Download This Cool Thing?
I'm glad you asked! You can download it from the Chrome Web Store: https://chrome.google.com/webstore/detail/what-i-see/ghcflgkkdehjaohkbnaeghpjboibdpkn
