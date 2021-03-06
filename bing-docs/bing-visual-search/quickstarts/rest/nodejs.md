---
title: "Quickstart: Get image insights using the REST API and Node.js - Bing Visual Search"
titleSuffix: Bing Search Services
description: Learn how to get image insights using Node.js and Bing Visual Search API.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Get image insights using the Bing Visual Search REST API and Node.js

Use this quickstart to make your first call to the Bing Visual Search API. This simple JavaScript application uploads an image to the API, and displays the information returned about it. Although this application is written in JavaScript, the API is a RESTful Web service compatible with most programming languages.

## Prerequisites

* [Node.js](https://nodejs.org/en/download/).
* The Request module for JavaScript. You can use `npm install request` command to install the module.
* The form-data module. You can use the `npm install form-data` command to install the module. 

<!--
[!INCLUDE [bing-visual-search-signup-requirements](../../../../includes/bing-visual-search-signup-requirements.md)]
-->

## Initialize the application

1. Create a JavaScript file in your favorite IDE or editor, and set the following requirements:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. Create variables for your API endpoint, subscription key, and the path to your image. 

    ```javascript
    var baseUri = 'https://api.bing.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. Create a function named `requestCallback()` to print the response from the API.

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## Construct and send the search request

1. When you upload a local image, the form data must include the `Content-Disposition` header. Set its `name` parameter to "image", and set the `filename` parameter to the file name of your image. The contents of the form include the binary data of the image. The maximum image size you can upload is 1 MB.

   ```
   --boundary_1234-abcd
   Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

   ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

   --boundary_1234-abcd--
   ```

2. Create a new `FormData` object with `FormData()`, and append your image path to it by using `fs.createReadStream()`.
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

3. Use the request library to upload the image, and call `requestCallback()` to print the response. Add your subscription key to the request header.

    ```javascript
    form.getLength(function(err, length){
      if (err) {
        return requestCallback(err);
      }
      var r = request.post(baseUri, requestCallback);
      r._form = form; 
      r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
    });
    ```

## Next steps

> [!div class="nextstepaction"]
> [Build a Visual Search single-page web app](../../tutorial/visual-search-single-page-app.md).
