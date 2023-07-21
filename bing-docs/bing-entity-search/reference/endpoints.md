---
title: Bing Entity Search API v7 Reference
titleSuffix: Bing Services
description: Describes the programming elements of the Bing Entity Search API.
ms.service: bing-search-services
ms.subservice: bing-entity-search
author: swhite-msft
manager: ehansen
ms.topic: reference
ms.date: 07/15/2020
ms.author: scottwhi
---

# Entity Search API v7 reference

The Entity Search API lets you send a search query to Bing and get back search results that include entities and places. Place results include restaurants, hotel, or other local businesses. For places, the query can specify the name of the local business or it can ask for a list (for example, restaurants near me). Entity results include persons, places (tourist attractions, states, countries/regions, etc.), or things. For examples that show how to make requests, see [Bing Entity Search overview](../overview.md). 

> [!NOTE]
> The Entity Search API supports only US business locations for Place answers. 

  
For information about permitted use and display of the results, see [Use and display requirements](../../bing-web-search/use-display-requirements.md).

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.
  
## Endpoints 
 
To request web search results, send a GET request to:  
  
```
https://api.bing.microsoft.com/v7.0/entities
```

The request must use the HTTPS protocol. 

> [!NOTE]
> The maximum URL length is 2,048 characters. To ensure that your URL length does not exceed the limit, the maximum length of your query parameters should be less than 1,500 characters. If the URL exceeds 2,048 characters, the server returns 404 Not found.  
  
## Next steps

Check out the following programming elements you'll use when sending search requests:

- [Headers](headers.md)
- [Query parameters](query-parameters.md)
- [Response objects](response-objects.md)
