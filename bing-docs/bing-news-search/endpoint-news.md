---
title: Bing News Search endpoints
titleSuffix: Bing Search Services
description: This article provides a summary of the News search API endpoints; news, top news, and trending news.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Bing News Search API endpoints


To get news search results using the Bing News Search API, send a `GET` request to one of the following endpoints. The headers and URL parameters define further specifications.

### News items by search query

```
GET https://api.bing.microsoft.com/bing/v7.0/news/search
```

Returns news items based on a search query. If the search query is empty, the API will return top news articles from different categories. Send a query by url encoding your search term and appending it to the`q=""` parameter. For supported markets, see [Market codes](reference/market-codes.md).

### Top news items by category

```
GET https://api.bing.microsoft.com/bing/v7.0/news  
```

Returns the top news items by category. You can specifically request the top business, sports, or entertainment articles using `category=business`, `category=sports`, or `category=entertainment`.  The `category` parameter can only be used with the `/news` URL. There are some formal requirements for specifying categories; refer to [category](reference/query-parameters.md#category) documentation. Send a query by url encoding your search term and appending it to the`q=""` parameter. For supported markets, see [Market codes](reference/market-codes.md).

### Trending news topics 

```
GET https://api.bing.microsoft.com/bing/v7.0/news/trendingtopics
```

Returns news topics that are currently trending on social networks. When the `/trendingtopics` option is included, Bing search ignores several other parameters, such as `freshness` and `?q=""`. For supported markets, see [Market codes](reference/market-codes.md).

## Next steps

For details about headers, parameters, market codes, response objects, errors, etc., see the [Bing News search API v7](reference/endpoints.md) reference.

For complete information about the parameters supported by each endpoint, see the reference pages for each type.
For examples of basic requests using the News search API, see [Bing News Search Quick-starts](quickstarts/rest/csharp.md).
