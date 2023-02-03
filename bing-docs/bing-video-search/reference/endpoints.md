---
title: Bing Video Search APIs v7 Reference
titleSuffix: Bing Services
description: Describes the programming elements of the Bing Video Search APIs.
ms.service: bing-search-services
ms.subservice: bing-video-search
author: alekhyasasi
manager: ehansen
ms.topic: reference
ms.date: 06/16/2022
ms.author: v-apunnamara
---

# Video Search APIs v7 reference

The Video Search APIs let you search for videos that are relevant to the user's search query, get insights about videos, and get trending videos. This section provides technical details about the query parameters and headers that you use to request videos and the JSON response objects that contain them. For examples that show how to make requests, see [Bing Video Search overview](../overview.md).
  
For information about permitted use and display of the results, see [Use and display requirements](../../bing-web-search/use-display-requirements.md).

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.
  
## Endpoints  

To request video search results, send a GET request to one of the following endpoints:  
  
|Endpoint|Description
|-|-
|`https://api.bing.microsoft.com/v7.0/videos/search`|Returns videos that are relevant to the user's search query.
|`https://api.bing.microsoft.com/v7.0/videos/details`|Returns insights about a video, such as related videos.
|`https://api.bing.microsoft.com/v7.0/videos/trending`|Returns videos that are trending based on search requests made by others. The videos are broken out into different categories. For example, Top Music Videos. For a list of markets that support trending videos, see [Supported trending videos markets](market-codes.md#trending-video-api-markets).

The request must use the HTTPS protocol.

> [!NOTE]
> The maximum URL length is 2,048 characters. To ensure that your URL length does not exceed the limit, the maximum length of your query parameters should be less than 1,500 characters. If the URL exceeds 2,048 characters, the server returns 404 Not found.  
  
## Next steps

Check out the following programming elements you'll use when sending search requests:

- [Headers](headers.md)
- [Query parameters](query-parameters.md)
- [Response objects](response-objects.md)
