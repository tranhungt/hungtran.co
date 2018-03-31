---
title: "Charles Proxy - Discovering Tinder's (Private) API"
date: 2014-08-03T18:48:15-05:00
draft: false
---

API calls are generally easy to discover, especially if you use web inspector tools like chrome's built in devtools when interacting with html pages.

But what if what you're trying to see what is embedded inside a native app? I was fascinated with how Tinder, the iPhone app, was trasmitting data and how to replicate it. There isn't a straightforward way of tracking that kind of network traffic without some type of traffic monitoring tool.

### Discovering the API with Charles Proxy
[Charles Proxy](http://www.charlesproxy.com/) is an http proxy service that allows you to monitor and manipulate all network traffic going in and out of your system. Upon initial set up, Charles will let you see all the network traffic on your computer, but we'll need to use its proxy tool in order to see the traffic coming through your iPhone.

1. To set up Charles Proxy, download it [here](http://www.charlesproxy.com/).
2. Download the [Charles SSL cert](http://www.charlesproxy.com/ssl.zip).
Since many requests on the net are sent through https, including tinder, you will need to allow your phone to accept the ssl cert from Charles.
3. For iOS: send the cert to yourself and open it with your device (iPhone).
4. Run Charles and turn on proxying.
a. In Charles, go to Proxy -> Proxy Settings.
b. Under 'Proxies', check 'Enabled transparent HTTP Proxying'. Port can be left at 8888.![](http://hungtranco_std.s3.amazonaws.com/2014/Aug/Captura_de_pantalla_2014_08_02_a_la_s__6_53_12_PM-1407032021533.png)
c. Under 'SSL', check 'Enable SSL Proxying' and add the location *:443. ![](http://hungtranco_std.s3.amazonaws.com/2014/Aug/Captura_de_pantalla_2014_08_02_a_la_s__6_52_53_PM-1407032083682.png)
5. Proxy your iOs device.
a. Make sure you're on the same local network.
b. Go to Wifi settings.
c. Tap the blue 'i' next to your connected network.
d. On the bottom, set 'HTTP PROXY' to 'Manual'
e. Set the 'Server' to your computer's local IP address, which can be found in your computer's 'Network' settings.
f. Set Port to 8888, which was the default set earlier.![](http://hungtranco_std.s3.amazonaws.com/2014/Aug/image-1407033321375.png)
e. Exit the wifi settings to save.


After your phone's proxy has been set up, Charles will ask you to allow your iOS device to proxy.

At this point, your device is properly proxied, and all requests going in and out of your device will be monitored by Charles.

Fire up the Tinder app, and you will see a series of requests going to https://api.gotinder.com.

When you're done, remember to:
1. Turn off Proxying on your phone.
2. Turn off Proxying on Charles.



### Creating a Nodejs Module to Wrap the API
All this discovery would go unnoticed and insignificant if it wasn't documented or somehow useable, so I decided to write a nodejs module that would encapsulate Tinder's API for consumption.

Read about it here:
http://hungtran.co/wrapping-up-tinders-private-api-into-a-nodejs-module/