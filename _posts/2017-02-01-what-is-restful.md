---
layout:     post
title:      "What is RESTful?"
subtitle:   ""
date:       2017-07-29
author:     "Kanon"
header-img: ""
catalog: false
tags:
    - HTTP
    - RESTful
---

Building RESTful web services, like other programming skills is part art, part science. As the Internet industry progresses, creating a REST API becomes more concrete with emerging best practices. As RESTful web services don't follow a prescribed standard except for HTTP, it's important to build your RESTful API in accordance with industry best practices to ease development and increase client adoption.

## What is RESTful
REST is short for Representational State Transfer.   
Briefly, RESTful is a style of creating web service, it has no technically constraints.  
But there  are a few recommended REST-like concepts. These six quick tips will result in better, more usable services.

## six practical ways to make your web RESTful
1.Use HTTP Verbs to Make Your Requests Mean Something

2.Provide Sensible Resource Names

3.Use HTTP Response Codes to Indicate Status

4.Create Fine-Grained Resources

5.Offer Both JSON and XML

6.Consider Connectedness

## 4 HTTP Verbs
- GET
- POST
- PUT
- DELETE

The biggest difference between POST and PUT is that PUT is idempotent while POST isn't idempotent.

## Common HTTP Response Status Codes
- **200 OK**<br>
General success status code. This is the most common code. Used to indicate success.

- **3XX**
Redirection

- **400 Bad Request**<br>
The request cannot be fulfilled due to bad syntax.

- **403 Forbidden**<br>
The request was a legal request, but the server is refusing to respond to it. 

- **404 Not Found**<br>
The requested resource could not be found but may be available again in the future. 

- **500 Internal Server Error**<br>
The server encountered an unexpected condition which prevented it from fulfilling the request
