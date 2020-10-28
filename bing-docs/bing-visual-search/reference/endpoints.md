---
title: Bing Visual Search API v7 Reference
titleSuffix: Bing Services
description: Describes the programming elements of the Bing Visual Search API.
ms.service: bing-search-services
ms.subservice: bing-visual-search
author: swhite-msft
manager: ehansen
ms.topic: reference
ms.date: 07/15/2020
ms.author: scottwhi
---

# Visual Search API v7 reference

The Visual Search API lets you send Bing an image and get back insights about it such as visually similar images, shopping sources, text recognition, identify entities (people, places, things), return other topical content for the user to explore, and more. This section provides technical details about the query parameters and headers that you use to request image insights and the JSON response objects that contain them. For examples that show how to make requests, see [Bing Visual Search overview](../overview.md). 
  
For information about permitted use and display of the results, see [Use and display requirements](../../bing-web-search/use-display-requirements.md).

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.
  
## Endpoints 
 
To request web search results, send a POST request to:  
  
```
https://api.bing.microsoft.com/v7.0/images/visualsearch
```

For information about the body of the POST, see [Get insights](../how-to/get-insights.md).

The request must use the HTTPS protocol. 

> [!NOTE]
> The maximum URL length is 2,048 characters. To ensure that your URL length does not exceed the limit, the maximum length of your query parameters should be less than 1,500 characters. If the URL exceeds 2,048 characters, the server returns 404 Not found.  
  
## Next steps

Check out the following programming elements you'll use when sending search requests:

- [Headers](headers.md)
- [Query parameters](query-parameters.md)
- [Response objects](response-objects.md)
