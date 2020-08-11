---
title: How to page through search results - Bing Search APIs
titleSuffix: Bing Search Services
description: Learn how to page through search results from the Bing Search APIs.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Paging search results

When you call any of the Bing APIs (for example, the Web Search API or Image Search API), the API returns a list of results. The list is a subset of the total number of results that may be relevant to the query. To get the estimated total number of available results, access the answer object's `totalEstimatedMatches` field.

The following example shows the `totalEstimatedMatches` field for News Search API.

```json
{
  "_type": "News",
  "readLink": "https://<host>/api/v7.0/news/search?q=Sports",
  "queryContext": {
    "originalQuery": "Sports"
  },
  "totalEstimatedMatches": 118000,
  "value": [. . .]
}
```

The estimated number of matches **is only an estimate** and may likely change from request to request.


## Paging through search results

To page through the results, use the *count* and *offset* query parameters.

The *count* parameter specifies the number of results to return in the response. The maximum number of results that you may request in the response is API specific (see [Count values by API](#count-values-by-api)). For example, the maximum count value that you may specify for the Image Search API is 150.

The *offset* parameter specifies the number of results to skip. The offset is zero-based and should be less than (`totalEstimatedMatches` - *count*).

If your user interface presents 20 news articles per page, set *count* to 20 and *offset* to 0 to get the first page of results. For each subsequent page, increment *offset* by 20 (for example, 20, 40).

The following shows an example that requests 20 news articles beginning at offset 40.

```
https://<host>/api/v7.0/news/search?q=sailing&count=20&offset=40&mkt=en-us
```

Because each API sets a default value for *count*, you may specify only *offset*. For example, if the News Search API’s default count is 20, you only need to include the *offset* query parameter.

```
https://<host>/api/v7.0/news/search?q=sailing&offset=40&mkt=en-us
```

### Count values by API

The following table list the default and maximum count value per API.

|API|Default|Maximum
|-|-|-
|Web Search|10|50
|Image Search|35|150
|News Search|10|100
|Video Search|35|105

For the Image, News, and Video APIs, paging applies to only the general search endpoint. For example, you may not use paging with the trending endpoints.


## Paging web search results

The Web Search API returns results that include webpages and may include other answers like images, videos, and news. When you page the search results, you are paging the webpage results and not the other answers. For example, if you set count to 15, Bing returns 15 webpage results, but may return 35 images and 4 news articles.

The answers that Bing returns from page to page is unknown. For example, Bing may include news on the first page but not the second page, or vise-versa.

Note that if you specify the *responseFilter* query parameter and do not include Webpages in the list of filters, you should not use the *count* and *offset* parameters.


## Paging images and video results

Typically, if you page 30 images at a time, you set the *offset* query parameter to 0 on your first request, and increment *offset* by 30 on each subsequent request. However, some results in the subsequent response may be duplicates of the previous response. For example, the first two images in the response may be the same as the last two images from the previous response.

To eliminate duplicate results, set the *offset* query parameter to the value in the `nextOffset` field of the [Images](../bing-image-search/reference/response-objects.md#images) object. The `nextOffset` value adjusts for duplicates.

```json
{
  "_type": "Images",
  "readLink": "images/search?q=nurburgring",
  "webSearchUrl": "https://www.bing.com/images/search?q=nurburgring&FORM=OIIARP",
  "queryContext": {. . .},
  "totalEstimatedMatches": 933,
  "pivotSuggestions": [. . .],
  "queryExpansion": [. . .],
  "relatedSearches": [. . .],
  "nextOffset": 65,
  "currentOffset": 0,
  "value": [. . .]
}
```

For example, if you want to page 30 images at a time, you'd set *count* to 30 and *offset* to 0 in your first request. In your next request, you'd set *count* to 30 and *offset* to the value of `nextOffset`. The value of `nextOffset` will be 30 if there are no duplicates or it may be 32 if there are 2 duplicates.

Use the same technique when paging videos.


## Next steps

- Learn about [using rank](rank-results.md) to display search results.
