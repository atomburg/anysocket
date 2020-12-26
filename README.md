# anysocket
Websocket forward

```javascript
var WebSocketServer = require('ws').Server; 

var Net = require('net'); 

var wss = new WebSocketServer({port: 8181}); 

var upstream = Net.connect({host: '10.59.97.214', port: 6379}, 
    function(){ console.log('Ok, connected to server'); 
}); 
var conn = null; 
wss.on('connection', function (_conn){ console.log('client connected'); 
    conn = _conn; 

    conn.on('message', function (message){ 
        console.log('[WS-R], received data'); 
        console.log(message); 
        console.log('[WS-T], forward to upstream'); 
        upstream.write(message); 
    }); 
}); 

upstream.on('data', function(data){ 
    console.log('[US-R], received data'); 
    console.log(data.toString()); 
    console.log('[US-T], response to downstream'); 
    conn.send(data); 
}); 

upstream.on('end', function(){ 
    console.log('Ok, close connection'); 
});
```
