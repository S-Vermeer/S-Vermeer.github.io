---
layout: page
title: API Calls with Javascript
permalink: /additional-research/API-Javascript
parent: Additional Research
grand_parent: Reference
---

### Context
To get access to the current fine dust data, we need to make a call to the API. This went fine when we were using Python to make these requests, however, when we want to implement it in our web app using Javascript, we get the following message:

```
Access to XMLHttpRequest at 'API CALL' from origin 'OUR WEB APP' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```


### CORS
#### What is it
CORS is an abbreviation for Cross-Origin Resource Sharing. It is an HTTP-header-based mechanism that allows a server to indicate any origins other than its own from which a browser should permit loading resources. 

What this means is that web applications can only request resources from the same origin as the application, unless that origin that the resources are requested from includes the right CORS headers. 
(https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)



#### Why does it exist
The main cause for the same-origin requirement is Cross-Site Request Forgery, or CSRF for short. This attack impersonates a trusted user and sends a website unwanted commands.
**CSRF** (Cross-Site Request Forgery) is an attack that impersonates a trusted user and sends a website unwanted commands. If someone adds extra parameters to a URL that alter the data behind a server, this is Cross-Site Request Forgery.
(https://developer.mozilla.org/en-US/docs/Glossary/CSRF)

### Problem
#### Why is it a problem for us
We are trying to request data from an API from a different origin than ours. We cannot alter the settings of the API ourselves, otherwise, we would be able to allow cross-origin communication or at least cross-origin communication from our domains. We need to be able to request the data to get the most accurate information about a location at that time. An alternative method could be to constantly scrape the data and save it and collect the latest record for a location, however, as we noticed during the scraping of the data the API stops working quite often which then, in turn, stops the script. We also don't want to overload the API with constant requests for new data. That is why we want direct access to the API, so we can get the latest data of a location and display it. However, the CORS protocol is preventing us from doing so.

#### What have we tried
##### Parameters
There are a variety of parameters that we can send with our AJAX request to alter what is sent. For example, using the datatype JSONP, or JSON with padding, can circumvent the CORS error. However, this requires the response to be of the JSONP data type. There are also a variety of settings such as crossOrigin, passing credentials etc. However, unless on the server side, the API passes a header that allows for cross-origin communication, these will all still trigger the CORS error.

##### Python
Seeing as it was working when we were using Python, another idea was to use Javascript to run a Python script. After a variety of methods of trying to run Python in Javascript, the one that worked was Pyodide. This module allows you to run a string of Python code in Javascript. However, the requests library, which we were using in our Python code originally, wasn't available in Pyodide. That is why we needed to install it using micropip, which was available. After installing it, it soon became apparent why it wasn't available. After using our original request code, we got the error:

`urllib3.exceptions.sslerror: can't connect to https url because the ssl module is not available`.

The reason why the requests module is not available is that it uses the operating system TCP Socket API. This API is not available in web browsers. This is due to the security restrictions that are imposed on all browser-based applications.
(https://stackoverflow.com/questions/72585785/caused-by-sslerrorcant-connect-to-https-url-because-the-ssl-module-is-not-av)

Different ways of fetching can be used, however, these all use the functionality that triggers the CORS error.

##### Proxy
Another option was to use a proxy. However, at this rate, it would be better to work on the API that was already most likely going to need to be created for this application.

##### Extension
Another option was to use an extension to simulate the CORS being added. In this case, we used the Google Chrome extension [Allow CORS: Access-Control-Allow-Origin](https://chrome.google.com/webstore/detail/allow-cors-access-control/lhobafahddgcelffkeicbaginigeejlf/related?hl=en). By default CORS is blocked for javascript APIs, which this feature unblocks on the browser it is installed on.

By enabling this extension for our page, we could test whether this was definitely the actual error, instead of hiding a different error. In our case, we are now able to make GET requests without a CORS error. This means that it is the actual error, but it is no fix as it is just a workaround, we cannot ask our users to install this extension just to use our website.

## Own API
### Why
Our own API functions as a kind of proxy between the front-end and the fine dust API. This means that we can use Python for the requests to the fine dust API, not triggering CORS, and either use the same origin or specify an allowed origin to use in our own API.

It also means that in the future we can easily make requests to our AI model. 

### Setup
For this API, we pair it with a port, and have it run specifically on there. Then we use Flask to allow for easy referencing and processing REST requests. 