---
title: "Chaos Monkey at Opentable"
date: 2018-01-11T19:04:44-05:00
draft: false
---

I’d like to share what I’ve been working on in the past month, as our team at Opentable has been focusing on making our services more resilient.


### Why?
At the heart of our team is a service that connects (almost) all of Opentable to the outside world via an API layer. And that means we depend on a lot of internal services - 14 direct dependencies to be exact. So that means at any one time, a particular dependency can be flapping or having connectivity issues. And so the worry is - exactly how stable is our system in case one of those services go down?

### What?
So I implemented [[Chaos Toolkit](https://github.com/chaostoolkit/chaostoolkit)], a tool that essentially allows us to look at what happens to our service when we simulate an outage of services that we rely on. 

### Ease of use
Utilizing the Chaos Toolkit, we can run off a configuration file that specifies 3 things:

1. Steady state hypothesis - Ensure that our service is running normally at steady state.
2. Action - Enact a fault, in this case, shutting down a particular service.
3. Probe - Check our service to see what, if anything has failed.


### Plusses 
Plus sides of this tool:

1. Easy to use - literally just 3 JSON fields (as mentioned above).
2. Language agnostic - runs in command line.
3. Can invoke any python function or command line processes.
4. Extensible via custom python modules.


### Insight
By utilizing this chaos tool with our automated integration tests, a few insights we’re able to see:

1. Our cache is actually working.
2. Exactly at what API endpoints our service is failing when a particular service is down.
3. Assurance that our service is indeed resilient in case of downstream failures.

### Limitations
As this is only a discovery tool, using this doesn't actually make your service more resilient, but provides valuable insight into failures and assurance to what is in fact resilient.



--- 
Thanks for reading,
_Hung_


---
__References:__

Chaos Toolkit
https://github.com/chaostoolkit/chaostoolkit

Principles of Chaos Engineerin
http://principlesofchaos.org/