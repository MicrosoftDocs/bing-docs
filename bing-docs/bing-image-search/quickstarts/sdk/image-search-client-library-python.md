---
title: Bing Image Search Python client library quickstart 
titleSuffix: Bing Search Services
description: The Image Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this Python quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: v-grvanp
---

# Quickstart: Use the Bing Image Search Python client library

Use this quickstart to make your first image search using the Bing Image Search client library, which is a wrapper for the API and contains the same features. This simple Python application sends an image search query, parses the JSON response, and displays the URL of the first image returned.

The source code for this sample is available [on GitHub](https://github.com/microsoft/bing-search-sdk-for-python/blob/main/samples/sdk/image-search-quickstart.py) with additional error handling and annotations.

## Prerequisites

* [Python 2.7 or 3.4](https://www.python.org/) and higher.

* The [Azure Image Search client library](https://pypi.org/project/azure-cognitiveservices-search-imagesearch/) for Python
    * Install using `pip install azure-cognitiveservices-search-imagesearch`

<!--
[!INCLUDE [bing-image-search-signup-requirements](../../../../includes/bing-image-search-signup-requirements.md)]
-->

## Create and initialize the application

1. Create a new Python script in your favorite IDE or editor, and the following imports:

    ```python
    from azure.cognitiveservices.search.imagesearch import ImageSearchClient
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Create variables for your subscription key and search term.

    ```python
    subscription_key = "Enter your key here"
    subscription_endpoint = "Enter your endpoint here"
    search_term = "canadian rockies"
    ```

## Create the image search client

1. Create an instance of `CognitiveServicesCredentials`, and use it to instantiate the client:

    ```python
    client = ImageSearchClient(endpoint=subscription_endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```
1. Send a search query to the Bing Image Search API:
    ```python
    image_results = client.images.search(query=search_term)
    ```
## Process and view the results

1. Parse the image results returned in the response.

If the response contains search results, store the first result and print out its details, such as a thumbnail URL, the original URL,along with the total number of returned images.  

```python
if image_results.value:
    first_image_result = image_results.value[0]
    print("Total number of images returned: {}".format(len(image_results.value)))
    print("First image thumbnail url: {}".format(
        first_image_result.thumbnail_url))
    print("First image content url: {}".format(first_image_result.content_url))
else:
    print("No image results returned!")
```

## Next steps

> [!div class="nextstepaction"]
> [Bing Image Search single-page app tutorial](../../tutorial/bing-image-search-single-page-app.md)

## See also

* [What is Bing Image Search?](../../overview.md)  
* [Python samples for the Bing Image Search SDK](https://github.com/microsoft/bing-search-sdk-for-python/blob/main/samples/sdk/image_search_samples.py)  
* [Bing Image Search API reference](../../reference/endpoints.md)

