---
title: Bing Image Search APIs v7 Reference
titleSuffix: Bing Services
description: Describes the programming elements of the Bing Image Search APIs.
ms.service: bing-search-services
ms.subservice: bing-image-search
author: alekhyasasi
manager: ehansen
ms.topic: reference
ms.date: 07/15/2020

---

# Image Search APIs v7 reference

The Image Search APIs let you search for images that are relevant to the user's search query and also get trending videos. This section provides technical details about the query parameters and headers that you use to request images and the JSON response objects that contain them. For examples that show how to make requests, see [Bing Image Search overview](../overview.md).
  
For information about permitted use and display of the results, see [Use and display requirements](../../bing-web-search/use-display-requirements.md).

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.
  
## Endpoints  

To request image search results, send a GET request to one of the following endpoints:  
  
|Endpoint|Description
|-|-
|<https://api.bing.microsoft.com/v7.0/images/search> | Returns images that are relevant to the user's search query.
|<https://api.bing.microsoft.com/v7.0/images/trending> | Returns images that are trending based on search requests made by others. The images are broken out into different categories. For example, Popular People Searches.<br/><br/>For a list of markets that support trending images, see [Supported trending images markets](market-codes.md#trending-image-api-markets).

The request must use the HTTPS protocol.

> [!NOTE]
> The maximum URL length is 2,048 characters. To ensure that your URL length does not exceed the limit, the maximum length of your query parameters should be less than 1,500 characters. If the URL exceeds 2,048 characters, the server returns 404 Not found.  
  
## Next steps

Check out the following programming elements you'll use when sending search requests:

- [Headers](headers.md)
- [Query parameters](query-parameters.md)
- [Response objects](response-objects.md)
