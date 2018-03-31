---
title: "Bookmarklet the Future of Front End Debugging"
date: 2014-10-08T19:03:23-05:00
draft: false
---

A bookmarklet is a bookmark stored in a web browser that, instead of linking to a different web page, would run javascript on the current page instead.

## Why is this important?

Working in a SaaS company, we support every one of our clients to get integrated with our software, which includes embedding a javascript snippet on their page to render our software. As many of you may have experience, this type of service can have many points of errors. Some are misconfigurations, while some are platform failures in disguise, and some are even human misunderstandings on what to expect.

When debugging on a foreign web page, or any web page, it's sometimes very difficult to scope out and collect all the information you need. Are the configurations set up correctly? Are API calls going to the right place? Is this reproducible anywhere else? Specifically which data points are affected, and what are their ids? And how do I test the issue on this page as it's rendered without jumping through all the hoops?


## What can a bookmarklet do?

When you have the ability to run avascript on a page, the entire world of web page manipulation opens up.

But the most powerful and interesting characteristic of a bookmarklet is that the injected javascript has the ability to interact within the same environment as all the javascript that's already on the page.

Bookmarklets enable you to extract data from the page and interact with it in a variety of ways. It could include a scraping script digging through all the DOM elements, or maybe a script extracting exposed global variables available from the page's existing javascript. These extracted data can then be presented as HTML via DOM manipulations.

With javascript at your fingertips, you can begin to surface all the hidden data lying underneath the page. Configuration settings can easily be found. Specific ids for misbehaving content can easily be tracked. And even relevant links can be included for easy navigation.

With a click of a button, you can get much of the manual tasks of digging for relevant information out of the way so you can begin on the actual debugging process.


## What else?
But maybe there's more than just extracting data already on the page.

With javascript, you can make calls to your API to manipulate data in real time. Perhaps there's a system cache that you'd like to refresh, or a queued job you'd like to put in front of the line so you can quickly work through possible failure points. Or maybe there's a request that the client failed to call that you'd like make manually. All of this is possible as long as there's an endpoint on your server that you can request.

Sure, much of this can be processed via the ORM in the back end, but a pre-filled request is much easier than re-typing all the data inside the console. More importantly, this kind of shortcut can be granted to someone who cannot access the ORM for whatever reason, whether it be a non-technical colleague, or even a customer.


## Common Problems I've Come Across
Having written a bookmarklet to assist in debugging client issues, these are some of the problems I've come across and my solutions to them.


### Getting around CORS
For security reasons, many requests from the browser to a cross domain server is prohibited due to CORS (cross origin resource sharing) restrictions, that is, a client on a.com cannot make an API call to b.com.

This behavior is common for many endpoints because the server generaly does not anticipate or allow a request from a cross domain client. 

To get around this, you can make a separate web server that will allow CORS requests and use it as a proxy to make requests to the restricted API.

### Authorized Requests
Perhaps these API endpoints use token authorization. Other than hard coding an access token to your requests, you can also open up an API endpoint that would give you a token based on an authenticated cookie session. This practice is common for services where authenticated sessions and tokens hold the same access permissions to authorized requests.

But you'd like to access this cookie programmatically from a third-party website. Naturally, a cross domain request would need to be ganted by a server, but you would still neeed to have the authentication necessary to retrieve the token.

With CORS, there's a special way to use the domain's cookie based on the domain you're requesting from. The server will need to include the headers `Access-Control-Allow-Credentials: true` and `Access-Control-Allow-Origin: '<origin requesting this api>'`, and the client ajax call include the header `xhrFields : {withCredentials: true}`. This technique essentially allows the browser to use the requested domain's cookie to access that domain's API. For example, a client on a.com is able to make a cross domain request to b.com using his cookie stored for b.com.

Now, with this API, you can retrieve the authorized token, which will allow you access to all authorized API calls, whether via CORS or with the server proxy.