---
title: "Long Polling and Websockets on Nodejs"
date: 2014-09-13T19:01:30-05:00
draft: false
---

In current web technology, there's a lot of seamless integration under the hood that average users do not notice or take for granted.
One example is how a web page is able to send and receive messages real-time, much like a chat application.

Information travel in two directions, that is, the client sends a message to the server, and the server sends a message to the client.
In the olden days, web pages had to be reloaded every time for a client to send or request new information to and from the server.

### Sending Data
Nowadays, to send a request to a server, it is very common to use AJAX to send an XMLHTTPRequest to the server without any page reloads. These requests are initiated by the client and would send specific form data to the server or request specific contents from the server.


### Polling
One way to solve this issue is by requesting the server periodically. This could be achieved by a simple `setInterval` function with an ajax call to the server every x seconds. This would be called polling, because we are constantly asking the server, 'do you have anything new?'.
This approach is very resource intensive, because there are a lot of traffic going back and forth, and especially heavy on the server when there are hundreds of thousands of clients requesting the server simultaenously. It is especially inefficient when there isn't any new data, yet all these requests are hitting the server and getting receiving empty responses.

### Push Technology - Event Based Programming
But what if the server wants to send the client new data that the client is interested in, but has no idea when the data would be available? This is sometimes referred to as push technology, where a server wants to send data to a client. A way to think about push technology is to only do something when there's a reason. That is, the server will only send a data response when there's data available.

### Long Polling
Long polling is where the client requests new data from the server, but the server does not respond until there is data. In the meantime, the client has an open connection to the server and is able to accept new data once the server has it ready to send.


To set up long polling on the client side, the client can set up an event loop, where, upon timeout or success of a request to the server, the client would continually request the server for new information, which would be known as a subscription event.
```
  subscribe: function(callback) {
      var longPoll = function(){
          $.ajax({
              method: 'GET',
              url: '/messages', 
              success: function(data){
                  callback(data)
              },
              complete: function(){
                  longPoll()
              },
              timeout: 30000
          })
      }
      longPoll()
  }
```



On the server side, the request is then subscribed to the message bus.
In Nodejs, the message bus is built in and can be required as follows. And you'll probably want to set max listeners to more than the default of 10.
```
var EventEmitter = require('events').EventEmitter
var messageBus = new EventEmitter()
messageBus.setMaxListeners(100)
```

Subscring the request.
```
router.get('/messages', function(req, res){
    var addMessageListener = function(res){
        messageBus.once('message', function(data){
            res.json(data)
        })
    }
    addMessageListener(res)
})
```


And so you're wondering, when will this message bus ever fire back data to the response?
Since we have a subscriber to the event, we also need a publisher to cause an event.



To emit a message to the message bus, the client, or something else, will need to initiate a publish event.
```
publish: function(data) {
    $.post('/messages', data)
}
```

And so the server can then emit a message to the message bus.
```
router.post('/messages', function(req, res){
    messageBus.emit('message', req.body)
    res.status(200).end()
})
```

In this set up, you ensure that every client continually has an open connection to the server, but the server is not easily overloaded to a multitude of requests, because it puts every request/response inside a message bus to be emitted once it is ready.
This method has been widely popular due to its relatively easy set up and wide support accross all browsers.
Though, long polling is only considered push-esque technology, because the client still needs to initate a request loop, but it is still able to achieve a push-like process.

### Web Sockets - A New Protocol
A relatively new approach to push technology is known as websockets, which is gaining support by most modern web browsers and web servers. Websocket is a full duplex TCP connection that is independent of the HTTP request and response cycle. Being full duplex, the client can send data to the server, and the server can send data to the client simultaneously without any http requests. This connection uses event based programming to both publish and subscribe to the socket for any changes in data.

#### Setting Up the Socket
On the server side, websocket support needs to be implemented. Since nodejs comes with websockets built in, we can require it off the main library. Also, websocket will, in a sense, inherit the http app server, because it requires the http server to interpret the handshake as an upgrade request.

```
app.set('port', process.env.PORT || 3000);
var server = app.listen(app.get('port'), function() {
  debug('Express server listening on port ' + server.address().port);
});

var messageBus = app.get('messageBus')
var wss = new WebSocketServer({server: server})
```

And to initiate the websocket connection from the server:
```
wss.on('connection', function(ws){
    //subscribe and publish to the socket to follow
})
```

To set up a a websocket connection from the client side:
```
setup: function(){
    this.socket = new WebSocket('ws://localhost:3000')
}
```



#### Sending messages to the Server
From the client side:
```
publish: function(data){
    this.socket.send(JSON.stringify(data))
}
```

From this publish event across the socket from the client, the server will emit a message to the message bus.
```
wss.on('connection', function(ws){
    ws.on('message', function(data){
        messageBus.emit('message', data)
    })
})
```

#### Pushing messages to the Client
Listening to this message bus, the server is then able to send a message back to the client via the socket:
```
var wsListener = function(data){
    ws.send(data)
}

messageBus.on('message', wsListener)
```


From the client's side, in order to receive this message from the server, it must also be listening to the socket for any incoming data.
```
subscribe: function(callback){
    this.socket.onmessage = function(event){
        callback(JSON.parse(event.data))
    }
}
```

#### Closing the Connection
For completeness, when the browser is closed, the server should also remove the event listener from the message bus.
```
ws.on('close', function(){
    messageBus.removeListener('message', wsListener)
})
```

Though there are many debates out there, websockets is considered very efficient because the lack of HTTP request headers being sent back and forth, wich results in lower traffic latency. And since websockets are much lighter weight, the server is able to handle significantly more websocket connections than http connections, allowing a much higher throughput in the amount of connections and data pushed to the client. The downside is that it is not fully supported across all popular borwsers, as IE 8 and 9 and a few others are left in the dark.


To check out a code sample using long polling and websockets, visit:
https://github.com/tranhungt/responsive_chatroom


