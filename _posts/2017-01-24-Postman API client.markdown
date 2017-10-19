---
layout: post
title:  "Postman API client"
date:   2017-01-24 16:49:39 +0100
comments: true  
categories: API development
---

Download Postman and ready to try.

# Table of contents
1. [Manage environment](#environment)  
2. [Set env parameter from response](#response)

## Manage environment <a name="environment"></a>

[Representational state transfer (REST) or RESTful Web services are one way of providing interoperability between computer systems on the Internet.](https://en.wikipedia.org/wiki/Representational_state_transfer)  
Postman can help with your Restful API development.  

To set up the environment your API uses, refer to this document.https://www.getpostman.com/docs/environments  


## Set env parameter from response <a name="response"></a>
GET request to get response body

This request returns a JSON body with a session token. For this dummy API, the token is needed for a successful POST request on the '/status' endpoint. To extract the token, we need the following code.


var jsonData = JSON.parse(responseBody);
postman.setEnvironmentVariable("token", jsonData.token);
```
var jsonData = JSON.parse(responseBody);
postman.setEnvironmentVariable("token", jsonData.token);
```
Add this to the test editor and hit send. Hover over the quick look window (q) to check that the variable "token" has the value extracted from the response.
http://blog.getpostman.com/2014/01/27/extracting-data-from-responses-and-chaining-requests/
