---
title: What is the Bing Video Search API?
titleSuffix: Bing Search Services
description: Learn how to search for videos across the web, using the Bing Video Search API.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-video-search
ms.topic: overview
ms.date: 07/15/2020
ms.author: scottwhi
---
# What is the Bing Video Search API?

The Bing Video Search API makes it easy to add video searching capabilities to your services and applications.  If you're looking to build an experience similar to [Bing Video](https://www.bing.com/videos), this is the API to use.


> [!NOTE]
> To comply with the new EU Copyright Directive in France, the Bing Web, News, Video, Image and all Custom Search APIs must omit some content from certain EU News sources for French users. The removed content may include thumbnail images and videos, video previews, and snippets which accompany search results from these sources. As a consequence, the Bing APIs may serve fewer results with thumbnail images and videos, video previews, and snippets to French users.


## Get started

To get started using the API, pick the subscription you want from <a href="https://aka.ms/bingsearchapipricing" target="_blank">Bing API Pricing</a>. After getting your subscription key, you're all set to make your first call. 

You can easily call the API by sending a native HTTP GET request or by using the Video Search SDK. For examples to help you get up and running quickly for either option, see [Quickstarts](quickstarts/quickstarts.md).


## Features  

Bing provides the following Video APIs:

- [Video Search API](how-to/get-videos.md), which you use to search the web for videos that the user requested.
- [Trending Video API](how-to/trending-videos.md), which you use the get videos that are trending.
- [Video Insights API](how-to/video-insights.md), which you use to get insights about a video such as related videos.

By default, Video Search API returns videos that the user requested. But if you want to have some control over what Bing returns, see the following search features:

|Feature|Description
|-|-
|[Filter the videos that bing returns](how-to/get-videos.md#filter-the-videos-that-bing-returns)|Filter videos by when Bing discovered them, that are embeddable, are free, and more.
|[Pivot user search queries](how-to/search-response.md#using-pivot-queries)|Get search strings that pivot on the user's search string.
|[Expand user search queries](how-to/search-response.md#using-expanded-queries)|Get search strings that expand on the user's search string.
|[Page results](../bing-web-search/page-results.md)|Page through multiple pages of video results.
|[Resize and crop thumbnails](../bing-web-search/resize-and-crop-thumbnails.md)|Resize or crop thumbnail images in the response to fit your experience.

Bing also provides API metrics, which you can use to inform your strategic decisions. Quickly retrieve statistics such as your top queries, call volume, market distribution, response code summary, and many more. For details, see [Bing Web Statistics](../bing-web-search/bing-web-stats.md).


### Search or search-like experience

Bing Video Search API may only be used as a result of a direct user query or search, or as a result of an action within an app or experience that logically can be interpreted as a user’s search request. For illustration purposes, the following are some examples of acceptable search or search-like experiences.

- User enters a query directly into a search box in an app.
- User selects specific text or image and requests “more information” or “additional information”.
- User asks a search bot about a particular topic.
- User dwells on a particular object or entity in a visual search type scenario.

If you are not sure if your experience can be considered a search-like experience, check with Microsoft.


## Next steps

- Learn about other APIs in the [family of Bing Search APIs](../bing-web-search/bing-api-comparison.md).
- Learn about [use and display requirements](../bing-web-search/use-display-requirements.md) for Bing Video Search.  
- Learn about [searching the web for videos](how-to/get-videos.md).
- Learn about what's in the [JSON response](how-to/search-response.md).
- Review [Video Search API v7 reference](reference/endpoints.md) documentation.  

