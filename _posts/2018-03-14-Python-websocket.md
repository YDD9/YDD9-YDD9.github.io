---
layout: post
title:  "Python Websocket-client"
date:   2018-03-14 14:51:39 +0100
comments: true
categories: Python websocket websocket-client
---

# websocket 
websocket is web protocol used to send/push data between server/client. It's built on TCP, it works well with HTTP, it uses the same port 80 and 443 as HTTP.
The reason websocket is there, server use websocket to push info to client which http can not. [More details](http://www.ruanyifeng.com/blog/2017/05/websocket.html)

In python, there exists two project, one is [websocket-client](https://github.com/websocket-client/websocket-client/blob/master/examples/echo_client.py), 
one is [websockets](https://websockets.readthedocs.io/en/stable/).
Pay attention to the name and ensure websocket-client module is this article talking about.

## installation
```
pip install websocket-client
```

## simple example to send message
`create_connection` is the one you should choose to use in such case, don't use `WebSocketApp` for such simple purpose
https://github.com/websocket-client/websocket-client/blob/master/examples/echo_client.py

You can add proxy, ssl config, customized header as json.
```
from __future__ import print_function
import websocket
imort ssl

if __name__ == "__main__":
    # will display details screen output for debug
    websocket.enableTrace(True)

    http_proxy_host = "Zscaler.proxy.corporate.com"
    http_proxy_port = 80

    header = {
            'predix-zone-id': "tsZoneID",
            'Authorization': 'Bearer accessToken',
            'content-type': 'application/json'
        }

    ws = websocket.create_connection("ws://echo.websocket.org/",
                                     http_proxy_host=http_proxy_host,
                                     http_proxy_port=http_proxy_port,
                                     sslopt={"cert_reqs": ssl.CERT_NONE},
                                     header=header
                                    )
    print("Sending 'Hello, World'...")
    ws.send("Hello, World")    # json.dumps(jsonObj)
    print("Sent")
    print("Receiving...")
    result = ws.recv()
    print("Received '%s'" % result)
    ws.close()
```

## simple application example
https://github.com/websocket-client/websocket-client/blob/master/examples/echoapp_client.py


## simple example to use `websocket.WebSocket`
Not sure the relation to the create_connection
```
ws = websocket.WebSocket(sslopt={"cert_reqs": ssl.CERT_NONE})
ws.connect("wss://echo.websocket.org")
```
