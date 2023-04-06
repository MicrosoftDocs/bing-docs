---
title: What is the Bing Image Search API?
titleSuffix: Bing Search Services
description: The Bing Image Search API enables you to use Bing's cognitive image search capabilities in your application. By sending user search queries with the API, you can get and display relevant and high-quality images similar to Bing Images.
services: bing-search-services
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
author: alekhyasasi
ms.date: 03/07/2023
---

# What is the Bing Image Search API?

Bing Image Search API lets your users find images on the world-wide-web. If you're looking to build an experience similar to [bing.com/images](https://www.bing.com/images), this is the API to use.

## Get started

To get started using the API, pick the subscription you want from [Bing API Pricing](https://aka.ms/bingsearchapipricing). After getting your subscription key, you're all set to make your first call.

You can easily call the API by sending a native HTTP GET request or by using the Image Search SDK. For examples to help you get up and running quickly for either option, see [Quickstarts](quickstarts/quickstarts.md).

## Features  

Bing provides the following Image APIs:

- [Image Search API](how-to/get-images.md), which you use to search the web for images that the user requested.
- [Trending Images API](how-to/trending-images.md), which you use the get images that are trending.

> [!NOTE]
> Use [Visual Search API](../bing-visual-search/overview.md) to get insights about an image.

By default, Image Search API returns images that the user requested. But if you want to have some control over what Bing returns, see the following search features:

|Feature|Description
|-|-
|[Filter the images that bing returns](how-to/get-images.md#filter-the-images-that-bing-returns)|Filter images by when Bing discovered them, by image type such as animated GIFs, by their license type, and more.
|[Pivot user search queries](how-to/search-response.md#using-pivot-queries)|Get search strings that pivot on the user's search string.
|[Expand user search queries](how-to/search-response.md#using-expanded-queries)|Get search strings that expand on the user's search string.
|[Page results](../bing-web-search/page-results.md)|Page through multiple pages of image results.
|[Resize and crop thumbnails](../bing-web-search/resize-and-crop-thumbnails.md)|Resize or crop thumbnail images in the response to fit your experience.

Bing also provides API metrics, which you can use to inform your strategic decisions. Quickly retrieve statistics such as your top queries, call volume, market distribution, response code summary, and many more. For details, see [Bing Web Statistics](../bing-web-search/bing-web-stats.md).

### Search or search-like experience

Bing Image Search API may only be used as a result of a direct user query or search, or as a result of an action within an app or experience that logically can be interpreted as a user’s search request. For illustration purposes, the following are some examples of acceptable search or search-like experiences.

- User enters a query directly into a search box in an app.
- User selects specific text or image and requests “more information” or “additional information”.
- User asks a search bot about a particular topic.
- User dwells on a particular object or entity in a visual search type scenario.

If you are not sure if your experience can be considered a search-like experience, check with Microsoft.

## Next steps

- Learn about other APIs in the [family of Bing Search APIs](../bing-web-search/bing-api-comparison.md).
- Learn about [use and display requirements](../bing-web-search/use-display-requirements.md) for Bing Image Search.  
- Learn about [searching the web for images](how-to/get-images.md).
- Learn about what's in the [JSON response](how-to/search-response.md).
- Review [Image Search API v7 reference](reference/endpoints.md) documentation.  
