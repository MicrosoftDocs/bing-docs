---
title: "Quickstart: Call your Bing Custom Search endpoint using Python | Microsoft Docs"
titleSuffix: Bing Search Services
description: Use this quickstart to begin requesting search results from your Bing Custom Search instance using Python.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Call your Bing Custom Search endpoint using Python

Use this quickstart to learn how to request search results from your Bing Custom Search instance. Although this application is written in Python, the Bing Custom Search API is a RESTful web service compatible with most programming languages. The source code for this sample is available on [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py).

## Prerequisites

- A Bing Custom Search instance. For more information, see [Quickstart: Create your first Bing Custom Search instance](../../how-to/quick-start.md).
- [Python](https://www.python.org/) 2.x or 3.x.

<!--
[!INCLUDE [bing-custom-search-prerequisites](../../../../includes/bing-custom-search-signup-requirements.md)]
-->

## Create and initialize the application

- Create a new Python file in your favorite IDE or editor, and add the following import statements. Create variables for your subscription key, custom configuration ID, and search term.

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## Send and receive a search request 

1. Construct the request URL by appending your search term to the `q=` query parameter, and your search instance's custom configuration ID to the `customconfig=` parameter. Separate the parameters with an ampersand (`&`). 

    ```python
    url = 'https://api.bing.microsoft.com/v7.0/custom/search?' + 'q=' + searchTerm + '&' + 'customconfig=' + customConfigId
    ```

2. Send the request to your Bing Custom Search instance, and print the returned search results.  

    ```python
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```

## Next steps

> [!div class="nextstepaction"]
> [Build a Custom Search web app](../../tutorial/custom-search-web-page.md)
