---
title: Sending requests to the Bing Spell Check API
titleSuffix: Bing Search Services
description: Learn about the Bing Spell Check modes, settings, and other information relating to the API.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Sending requests to the Bing Spell Check API

To check a text string for spelling and grammar errors, you'd send a GET request to the following endpoint:  

```
https://api.bing.microsoft.com/bing/v7.0/spellcheck
```  
  
The request must use the HTTPS protocol.

We recommend that all requests originate from a server. Distributing the key as part of a client application provides more opportunity for a malicious third-party to access it. A server also provides a single upgrade point for future versions of the API.

The request must specify the [text](../reference/query-parameters.md#text) query parameter, which contains the text string to proof. Although optional, the request should also specify the [mkt](../reference/query-parameters.md#mkt) query parameter, which identifies the market where you want the results to come from. For a list of optional query parameters such as `mode`, see [Query Parameters](../reference/query-parameters.md). All query parameter values must be URL encoded.  
  
The request must specify the [Ocp-Apim-Subscription-Key](../reference/headers.md#subscriptionkey) header. Although optional, you are encouraged to also specify the following headers. These headers help the Bing Spell Check API return more accurate results:  
  
-   [User-Agent](../reference/headers.md#useragent)  
-   [X-MSEdge-ClientID](../reference/headers.md#clientid)  
-   [X-Search-ClientIP](../reference/headers.md#clientip)  
-   [X-Search-Location](../reference/headers.md#location)  

For a list of all request and response headers, see [Headers](../reference/headers.md).

When calling the Bing Spell Check API using JavaScript, your browser's built-in security features might prevent you from accessing the values of these headers.

To resolve this issue, you can make the Bing Spell Check API request through a CORS proxy. The response from such a proxy has a `Access-Control-Expose-Headers` header that identifies response headers and makes them available to JavaScript.

It's easy to install a CORS proxy to allow the [tutorial app](../tutorial/spellcheck.md) to access the optional client headers. First, if you don't already have it, [install Node.js](https://nodejs.org/en/download/). Then enter the following command at a command prompt.

```console
npm install -g cors-proxy-server
```

Next, change the Bing Spell Check API endpoint in the HTML file to:
`http://localhost:9090/https://api.bing.microsoft.com/bing/v7.0/spellcheck/`

Finally, start the CORS proxy with the following command:

```console
cors-proxy-server
```

Leave the command window open while you use the tutorial app; closing the window stops the proxy. In the expandable HTTP Headers section below the search results, you can now see the `X-MSEdge-ClientID` header (among others) and verify that it's the same for each request.

## Example API request

The following shows a request that includes all the suggested query parameters and headers. If it's your first time calling any of the Bing APIs, don't include the client ID header. Only include the client ID if you've previously called a Bing API and Bing returned a client ID for the user and device combination. 
  
```http
GET https://api.bing.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

The following shows the response to the previous request. The example also shows the Bing-specific response headers.

[!INCLUDE [bing-url-note](../../../includes/bing-url-note.md)]

```json
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  

## Next steps

- [What is the Bing Spell Check API?](../overview.md)
- [Bing Spell Check API v7 Reference](../reference/endpoints.md)