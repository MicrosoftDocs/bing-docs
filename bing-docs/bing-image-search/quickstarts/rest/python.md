---
title: "Quickstart: Search for images using the Bing Image Search REST API and Python"
titleSuffix: Bing Search Services
description: Use this quickstart to send image search requests to Bing Image Search REST API using Python.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Search for images using Bing Image Search REST API and Python

Use this quickstart to learn how to send search requests to Bing Image Search API. This Python application sends a search query to the API, and displays the URL of the first image in the results. Although this application is written in Python, the API is a RESTful web service compatible with most programming languages.

To run this example as a Jupyter Notebook on [MyBinder](https://mybinder.org), select the **launch binder** badge:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)


The source code for this sample is available [on GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingImageSearchv7.py) with additional error handling and annotations.


## Prerequisites

* [Python 2.x or 3.x](https://www.python.org/)
* The [Python Imaging Library (PIL)](https://pillow.readthedocs.io/en/stable/index.html)
* [matplotlib](https://matplotlib.org/) 

<!--
[!INCLUDE [bing-image-search-signup-requirements](../../../../includes/bing-image-search-signup-requirements.md)]
-->

## Create and initialize the application

1. Create a new Python file in your favorite IDE or editor, and import the following modules. Create a variable for your subscription key, search endpoint, and search term. 

    ```python
    import requests
    import matplotlib.pyplot as plt
    from PIL import Image
    from io import BytesIO
    
    subscription_key = "your-subscription-key"
    search_url = "https://api.bing.microsoft.com/v7.0/images/search"
    search_term = "puppies"
    ```

2. Add your subscription key to the `Ocp-Apim-Subscription-Key` header by creating a dictionary, and adding the key as a value. 

    ```python
    headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
    ```

## Create and send a search request

1. Create a dictionary for the search request's parameters. Add your search term to the `q` parameter. Set the `license` parameter to `public` to search for images in the public domain. Set the `imageType` to `photo` to search only for photos.

    ```python
    params  = {"q": search_term, "license": "public", "imageType": "photo"}
    ```

2. Use the `requests` library to call the Bing Image Search API. Add your header and parameters to the request, and return the response as a JSON object. Get the URLs to several thumbnail images from the response's `thumbnailUrl` field.

    ```python
    response = requests.get(search_url, headers=headers, params=params)
    response.raise_for_status()
    search_results = response.json()
    thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
    ```

## View the response

1. Create a new figure with four columns and four rows by using the matplotlib library. 

2. Iterate through the figure's rows and columns, and use the PIL library's `Image.open()` method to add an image thumbnail to each space. 

3. Use `plt.show()` to draw the figure and display the images.

    ```python
    f, axes = plt.subplots(4, 4)
    for i in range(4):
        for j in range(4):
            image_data = requests.get(thumbnail_urls[i+4*j])
            image_data.raise_for_status()
            image = Image.open(BytesIO(image_data.content))        
            axes[i][j].imshow(image)
            axes[i][j].axis("off")
    plt.show()
    ```


## Example JSON response

Responses from the Bing Image Search API are returned as JSON. This sample response has been truncated to show a single result.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }]
}
```

## Next steps

> [!div class="nextstepaction"]
> [Bing Image Search single-page app tutorial](../../tutorial/bing-image-search-single-page-app.md)

## See also

* [What is Bing Image Search?](../../overview.md)  
* [Bing Image Search API reference](../../reference/endpoints.md)
