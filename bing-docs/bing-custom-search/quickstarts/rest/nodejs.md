---
title: "Quickstart: Call your Bing Custom Search endpoint using Node.js | Microsoft Docs"
titleSuffix: Bing Search Services
description: Use this quickstart to begin requesting search results from your Bing Custom Search instance using Node.js.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Call your Bing Custom Search endpoint using Node.js

Use this quickstart to learn how to request search results from your Bing Custom Search instance. Although this application is written in JavaScript, the Bing Custom Search API is a RESTful web service compatible with most programming languages. The source code for this sample is available on [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingCustomSearchv7.js).

## Prerequisites

- A Bing Custom Search instance. For more information, see [Quickstart: Create your first Bing Custom Search instance](../../how-to/quick-start.md).

- [The Node.js JavaScript runtime](https://www.nodejs.org/).

- The [JavaScript request library](https://github.com/request/request).

<!--
[!INCLUDE [bing-custom-search-prerequisites](../../../../includes/bing-custom-search-signup-requirements.md)]
-->

## Create and initialize the application

- Create a new JavaScript file in your favorite IDE or editor, and add a `require()` statement for the requests library. Create variables for your subscription key, custom configuration ID, and search term.

    ```javascript
    var request = require("request");
    
    var subscriptionKey = 'YOUR-SUBSCRIPTION-KEY';
    var customConfigId = 'YOUR-CUSTOM-CONFIG-ID';
    var searchTerm = 'microsoft';
    ```

## Send and receive a search request 

1. Create a variable to store the information being sent in your request. Construct the request URL by appending your search term to the `q=` query parameter, and your search instance's custom configuration ID to the `customconfig=` parameter. Separate the parameters with an ampersand (`&`). 

    ```javascript
    var info = {
        url: 'https://api.bing.microsoft.com/v7.0/custom/search?' + 
            'q=' + searchTerm + "&" +
            'customconfig=' + customConfigId,
        headers: {
            'Ocp-Apim-Subscription-Key' : subscriptionKey
        }
    }
    ```

1. Use the JavaScript request library to send a search request to your Bing Custom Search instance and print information about the results, including its name, url, and the date the webpage was last crawled.

    ```javascript
    request(info, function(error, response, body){
            var searchResponse = JSON.parse(body);
            for(var i = 0; i < searchResponse.webPages.value.length; ++i){
                var webPage = searchResponse.webPages.value[i];
                console.log('name: ' + webPage.name);
                console.log('url: ' + webPage.url);
                console.log('displayUrl: ' + webPage.displayUrl);
                console.log('snippet: ' + webPage.snippet);
                console.log('dateLastCrawled: ' + webPage.dateLastCrawled);
                console.log();
            }
    ```

## Next steps

> [!div class="nextstepaction"]
> [Build a Custom Search web app](../../tutorial/custom-search-web-page.md)
