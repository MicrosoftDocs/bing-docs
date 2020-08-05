---
title: Bing Autosuggest API v7 Reference
titleSuffix: Bing Search Services
description: Describes the programming elements of the Bing Autosuggest API.
ms.service: bing-search-services
ms.subservice: bing-autosuggest
author: swhite-msft
manager: ehansen
ms.topic: reference
ms.date: 07/15/2020
ms.author: scottwhi
---

# Autosuggest API v7 reference

The Autosuggest API lets you send a partial search query term to Bing and get back a list of suggested queries that other users have searched on or that are based on user intent. Typically, you use this API to support a richer search box experience. For example, you'd call this API for each character the user enters and populate the search box's drop-down list with the suggested query strings.  

For examples that show how to make requests, see [Getting suggested search terms](../overview.md). 
  
For information about permitted use and display of the results, see [Use and display requirements](../../bing-web-search/use-display-requirements.md).

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.
  
## Endpoints 
 
To request web search results, send a GET request to:  
  
```
https://api.bing.microsoft.com/bing/v7.0/suggestions
```

The request must use the HTTPS protocol. 

> [!NOTE]
> The maximum URL length is 2,048 characters. To ensure that your URL length does not exceed the limit, the maximum length of your query parameters should be less than 1,500 characters. If the URL exceeds 2,048 characters, the server returns 404 Not found.  
  
## Next steps

Check out the following programming elements you'll use when sending search requests:

- [Headers](headers.md)
- [Query parameters](query-parameters.md)
- [Response objects](response-objects.md)
