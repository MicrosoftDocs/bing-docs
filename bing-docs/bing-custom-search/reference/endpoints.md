---
title: Bing Custom Web Search API v7 Reference
titleSuffix: Bing Services
description: Describes the programming elements of the Bing Custom Web Search API.
ms.service: bing-search-services
ms.subservice: bing-custom-search
author: swhite-msft
manager: ehansen
ms.topic: reference
ms.date: 07/15/2020
ms.author: scottwhi
---

# Custom Web Search API v7 reference

The Web Search API lets you send a search query to Bing and get back web pages from the slice of Web that your Custom Search instance defines. This section provides technical details about the query parameters and headers that you use to request web results and the JSON response objects that contain them. For information about configuring a Custom Search instance, see [Configure your custom search experience](../overview.md). 
  
For information about permitted use and display of the results, see [Use and display requirements](../../bing-web-search/use-display-requirements.md).

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.
  
## Endpoints 
 
To request search results from your custom instance, send a GET request to:  
  
|Endpoint|Service
|-|-
|`https://api.bing.microsoft.com/v7.0/custom/search`|Custom Web Search
|`https://api.bing.microsoft.com/v7.0/custom/suggestions/search`|Custom Autosuggest
|`https://api.bing.microsoft.com/v7.0/custom/images/search`|Custom Image Search
|`https://api.bing.microsoft.com/v7.0/custom/videos/search`|Custom Video Search

The request must use the HTTPS protocol. 


> [!NOTE]
> The maximum URL length is 2,048 characters. To ensure that your URL length does not exceed the limit, the maximum length of your query parameters should be less than 1,500 characters. If the URL exceeds 2,048 characters, the server returns 404 Not found.  
  
## Next steps

Check out the following programming elements you'll use when sending search requests:

- [Headers](headers.md)
- [Query parameters](query-parameters.md)
- [Response objects](response-objects.md)
