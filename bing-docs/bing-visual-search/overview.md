---
title: What is the Bing Visual Search API?
titleSuffix: Bing Search Services
description: Bing Visual Search provides details or insights about an image such as similar images or shopping sources.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-visual-search
ms.topic: overview
ms.date: 01/18/2023
ms.author: v-apunnamara
---

# What is the Bing Visual Search API?

Bing Visual Search API returns insights about an image. For example, Bing can help you find similar images, learn where to buy the dress seen in the pic, explore a landmark, identify a dog’s breed, and more. All you need is an image file, the URL to an image, or an insights token. Use Bing Visual Search API to offer an experience similar to Bing.com/visualsearch.

> [!IMPORTANT]
> Bing Visual Search API is the preferred method for getting image insights. If you use [Image Insights API](../bing-image-search/how-to/image-insights.md), consider switching to Bing Visual Search API.

## Get started

To get started using the API, pick the subscription you want from [Bing API Pricing](https://aka.ms/bingsearchapipricing). After getting your subscription key, you're all set to make your first call.

You can easily call the API by sending a native HTTP GET request or by using the Visual Search SDK. For examples to help you get up and running quickly for either option, see [Quickstarts](quickstarts/quickstarts.md).

## Features  

You can discover the following insights by using Bing Visual Search:

|Insight|Description
|-|-
|Visually similar images|A list of images that are visually similar to the source image.
|Shopping sources|A list of places where you can buy the item seen in the image.
|Related searches|A list of related searches made by others or that are based on the contents of the image.
|Webpages that include the image|A list of webpages that include the image.
|Entities|A list of well-known people, places, and things seen in the image.

Bing Visual Search results also include bounding boxes for regions of interest in the image. For example, if the image contains several celebrities, the results may include bounding boxes for each of the recognized celebrities. Or, if Bing recognizes a product or clothing in the image, the result may include a bounding box for the recognized item. You can use the bounding box to explore more about its contents.

Bing provides API metrics, which you can use to inform your strategic decisions. Quickly retrieve statistics such as your top queries, call volume, market distribution, response code summary, and many more. For details, see [Bing Web Statistics](../bing-web-search/bing-web-stats.md).

### Search or search-like experience

Bing Visual Search API may only be used as a result of a direct user query or search, or as a result of an action within an app or experience that logically can be interpreted as a user’s search request. For illustration purposes, the following are some examples of acceptable search or search-like experiences:

- User enters a query directly into a search box in an app.
- User selects specific text or image and requests “more information” or “additional information”.
- User asks a search bot about a particular topic.
- User dwells on a particular object or entity in a visual search type scenario.

If you are not sure if your experience can be considered a search-like experience, check with Microsoft.

## Next steps

- Learn about other APIs in the [family of Bing Search APIs](../bing-web-search/bing-api-comparison.md).
- Learn about [use and display requirements](../bing-web-search/use-display-requirements.md) for Bing Visual Search.  
- Learn about how to [get image insights](how-to/get-insights.md).
- Learn about what's in the [JSON response](how-to/search-response.md).
- Review [Visual Search API v7 reference](reference/endpoints.md) documentation.  
