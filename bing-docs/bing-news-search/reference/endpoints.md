---
title: Bing News Search APIs v7 Reference
titleSuffix: Bing Services
description: Describes the programming elements of the Bing News Search APIs.
ms.service: bing-search-services
ms.subservice: bing-news-search
author: alekhyasasi
manager: ehansen
ms.topic: reference
ms.date: 07/07/2023
---

# News Search APIs v7 reference

The News Search APIs let you get a list of relevant news articles to a user's query, get top news articles, and get trending news. This section provides technical details about the query parameters and headers that you use to request news articles and the JSON response objects that contain them. For examples that show how to make requests, see [Bing News Search overview](../overview.md).
  
For information about permitted use and display of the results, see [Use and display requirements](../../bing-web-search/use-display-requirements.md).

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.
  
## Endpoints  

To request news search results, send a GET request to one of the following endpoints:  
  
|Endpoint|Description
|-|-
|<https://api.bing.microsoft.com/v7.0/news>|Returns the top news articles by category. For example, you can request the top sports or entertainment articles. For information about specifying categories, see the [category](query-parameters.md#category) query parameter.
|<https://api.bing.microsoft.com/v7.0/news/search>|Returns news articles based on the user's search query. If the search query is empty, the call returns the top news articles.
|<https://api.bing.microsoft.com/v7.0/news/trendingtopics>|Returns news topics that are currently trending on social networks.<br/><br/>For a list of markets that support trending news, see [Supported Trending News markets](market-codes.md#trending-news-api-markets).

The request must use the HTTPS protocol.

> [!NOTE]
> The maximum URL length is 2,048 characters. To ensure that your URL length does not exceed the limit, the maximum length of your query parameters should be less than 1,500 characters. If the URL exceeds 2,048 characters, the server returns 404 Not found.  
  
## Next steps

Check out the following programming elements you'll use when sending search requests:

- [Headers](headers.md)
- [Query parameters](query-parameters.md)
- [Response objects](response-objects.md)
