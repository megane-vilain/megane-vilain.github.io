---
layout: post
title : Http Error Code
author: Mégane Vilain
category: Other
---

## 1XX Informational response

|Code|Meaning|
|---|---
100 - Continue|The server has received the request headers and the client should proceed to send the request body 

## 2XX Successful responses

|Code|Meaning|
|---|---|
200 - OK|Everything is working as expected 
|204 - No Content|The server successfully processed the request, and is not returning any content

## 3XX Redirection messages

|Code|Meaning|
|---|---|
301 - Moved Permanently| Ressource was permanently moved to a new location. User will be redirected
302 - Found (Temporary Redirect) | Ressource was moved but it's only temporary

## 4XX Client error responses

|Code|Meaning|
|---|---|
404 - Not Found| Server couldn't find the requested ressource

## 5XX Server error responses

|Code|Meaning|
|---|---|
500 - Internal Server Error| Something unexpected happned on the server
502 Bad Gateway|This happens when one server, acting as a gateway or proxy, receives a faulty response from an upstream server. It often indicates an issue with the main server.
503 Service Unavailable| This code shows the server is temporarily unable to handle the request. It might be down for maintenance or experiencing high traffic but should be back soon.


