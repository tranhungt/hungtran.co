---
title: "Mapping Hostnames With Nginx"
date: 2018-03-27T19:12:34-05:00
draft: true
---

### What a Hosts File Does
A hosts file holds the name and addresses of web domains so it does not have to have to ask the DNS server to translate domain names to IP addresses. When you make an http request in the browser, the browser can ask your computer for the IP translation, and that can be done right through your local hosts file instead of asking a remote DNS server. This is especially helpful when developing local web services or when DNS servers are failing and you need a way to access websites, as in the case of the Dyn DNS DDOS attack on Oct 21.


### Mapping Hostnames with Hosts File

When developing or debugging locally, sometimes it's necessary to be able to make requests using the deployed hostname to see how your application will act. This is especially important when the application acts as a passthrough and you want to preserve the hostname to downstream services.

Let's say you want to call www.example.com in your browser, but have it mapped to your server running on localhost:8000.


### SSL With NGINX


https://www.digitalocean.com/community/tutorials/how-to-create-an-ssl-certificate-on-nginx-for-ubuntu-14-04

http://blog.getpostman.com/2014/01/28/using-self-signed-certificates-with-postman/

