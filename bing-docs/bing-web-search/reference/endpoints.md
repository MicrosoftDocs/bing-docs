---
title: Bing Web Search API v7 Reference
titleSuffix: Bing Services
description: Describes the programming elements of the Bing Web Search API.
ms.service: bing-search-services
ms.subservice: bing-web-search
author:  swhite-msft
manager: ehansen
ms.topic: reference
ms.date: 07/15/2020
ms.author: scottwhi
---
> [!WARNING] 
> **Product to be retired** Bing Search and Bing Custom Search APIs will be retired on 11th August 2025. 
> New deployments are not available and existing resources will be disabled. [Learn more](https://aka.ms/BingAPIsRetirement)
<br/>

# Web Search API v7 reference

The Web Search API lets you send a search query to Bing and get back search results that include links to webpages, images, and more. This section provides technical details about the query parameters and headers that you use to request web search results and the JSON response objects that contain them. For examples that show how to make requests, see [Bing Web Search overview](../overview.md). 
  
For information about what you can do with the search results, see [Terms of Use and Use and Display Requirements](https://aka.ms/BingAPIsLegal).

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.
  
## Endpoints 
 
To request web search results, send a GET request to:  
  
```
https://api.bing.microsoft.com/v7.0/search
```

The request must use the HTTPS protocol. 

> [!NOTE]
> The maximum URL length is 2,048 characters. To ensure that your URL length does not exceed the limit, the maximum length of your query parameters should be less than 1,500 characters. If the URL exceeds 2,048 characters, the server returns 404 Not found.  
  
## Next steps

Check out the following programming elements you'll use when sending search requests and processing the responses:

- [Headers](headers.md)
- [Query parameters](query-parameters.md)
- [Response objects](response-objects.md)
