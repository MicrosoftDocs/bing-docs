---
title:  "Quickstart: Send a search request to the REST API using Node.js - Bing Entity Search"
titleSuffix: Bing Search Services
description: Use this quickstart to send a request to the Bing Entity Search REST API using Node.js.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Send a search request to Bing Entity Search REST API using Node.js

Use this quickstart to make your first call to Bing Entity Search API and view the JSON response. This simple JavaScript application sends a news search query to the API, and displays the response. The source code for this sample is available on [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingEntitySearchv7.js).

Although this application is written in JavaScript, the API is a RESTful Web service compatible with most programming languages.

## Prerequisites

* The latest version of [Node.js](https://nodejs.org/en/download/).

* The [JavaScript Request Library](https://github.com/request/request).

<!--
[!INCLUDE [bing-news-search-signup-requirements](../../../../includes/bing-entity-search-signup-requirements.md)]
-->

## Create and initialize the application

1. Create a new JavaScript file in your favorite IDE or editor, and set the strictness and HTTPS requirements.

    ```javaScript
    'use strict';
    let https = require ('https');
    ```

2. Create variables for the API endpoint, your subscription key, and search query. 

    ```javascript
    let subscriptionKey = 'ENTER YOUR KEY HERE';
    let host = 'api.bing.microsoft.com';
    let path = '/v7.0/entities';
    
    let mkt = 'en-US';
    let q = 'italian restaurant near me';
    ```

3. Append your market and query parameters to a string called `query`. Be sure to url-encode your query with `encodeURI()`.
    ```javascript 
    let query = '?mkt=' + mkt + '&q=' + encodeURI(q);
    ```

## Handle and parse the response

1. Define a function named `response_handler()` that takes an HTTP call, `response`, as a parameter. 

2. Within this function, define a variable to contain the body of the JSON response.  
    ```javascript
    let response_handler = function (response) {
        let body = '';
    };
    ```

3. Store the body of the response when the `data` flag is called.
    ```javascript
    response.on('data', function (d) {
        body += d;
    });
    ```

4. When an `end` flag is signaled, parse the JSON, and print it.

    ```javascript
    response.on ('end', function () {
    let json = JSON.stringify(JSON.parse(body), null, '  ');
    console.log (json);
    });
        ```

## Send a request

1. Create a function called `Search()` to send a search request. 

2. Within this function, create a JSON object containing your request parameters. Use `Get` for the method, and add your host and path information. Add your subscription key to the `Ocp-Apim-Subscription-Key` header. 

3. Use `https.request()` to send the request with the response handler created previously, and your search parameters.
    
   ```javascript
   let Search = function () {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path + query,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };
    
    let req = https.request (request_params, response_handler);
    req.end ();
   }
      ```

2. Call the `Search()` function.

## Example JSON response

A successful response is returned in JSON, as shown in the following example: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## Next steps

> [!div class="nextstepaction"]
> [Build a single-page web app](../../tutorial/bing-entities-search-single-page-app.md)

* [What is the Bing Entity Search API?](../../overview.md)
* [Bing Entity Search API reference](../../reference/endpoints.md).
