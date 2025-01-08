---
title: Get news that's trending on social networks
titleSuffix: Bing Search Services
description: Learn how to get news that's trending on social networks
services: bing-search-services
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: conceptual
author: alekhyasasi
ms.date: 07/07/2023
---

# Get news that's trending on social networks

If you’re building a news-only search results page and you want to get news that’s trending on social media, call this API.

Calling the API is easy. If you have your subscription key, just send an HTTP GET request to the following endpoint:

`<https://api.bing.microsoft.com/v7.0/news/trendingtopics>`

Here's a cURL example that shows you how to call the endpoint using your subscription key:

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news/trendingtopics
```

## Request and response headers

Although that's all you need to do to get trending news, Bing suggests that you include a couple of other headers to provide a better search experience for your user. Those headers include:

- User-Agent &mdash; Lets Bing know whether needs a mobile or desktop experience.
- X-MSEdge-ClientID &mdash; Provides continuity of experience.
- X-MSEdge-ClientIP &mdash; Provides the user's location for location aware queries.
- X-Search-Location &mdash; Provides the user's location for location aware queries.

The more information you can provide Bing, the better the search experience will be for your users. To learn more about these headers, see [Request headers](../reference/headers.md#request-headers).

Here's a cURL example that includes these headers.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -H "X-MSEdge-ClientID: 00B4230B74496E7A13CC2C1475056FF4" -H "X-MSEdge-ClientIP: 11.22.33.44" -H "X-Search-Location: lat:55;long:-111;re:22" -A "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.65 Safari/537.36" https://api.bing.microsoft.com/v7.0/news/trendingtopics?mkt=en-us
```

Bing returns a couple of headers you should capture.

- BingAPIs-TraceId &mdash; ID that identifies the request in the log file.
- X-MSEdge-ClientID &mdash; The ID that you need to pass in subsequent request to provide continuity of experience.
- BingAPIs-Market &mdash; The market used by Bing for the request.

To learn more about these headers, see [Response headers](../reference/headers.md#response-headers).

Here's a cURL call that returns the response headers. If you want to remove the response data so you can see only the headers, include the `-o nul` parameter.

```curl
curl -D - -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news/trendingtopics?mkt=en-us
```

## Query parameters

The API supports a number of query parameters that you can pass in your request. Here's a list of the ones you're most likely to use.

- *mkt* &mdash; Used to specify the market where the results come from, which is typically the market where the user is making the request from. For a list of markets that this API supports, see [Trending News API markets](../reference/market-codes.md#news-category-api-markets).
- *since* &mdash; Used to select trending topics that Bing discovered on or after the specified date and time.
- *sortBy* &mdash; Use to specify the order (by date or by relevance) to return news topics in.

To learn more about these parameters, see [Query parameters](../reference/query-parameters.md).

Here's a cURL example that includes these query parameters.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news/trendingtopics?mkt=en-us&since=1597755615&sortby=date
```

## What the response looks like

The following JSON fragment shows what the Trending News response looks like. Use `name` and `image` to present the list of trending news stories. Be sure to use `provider` to attribute the image. To make the `name` and/or `image` clickable, do one of the following:

- Use the URL in `webSearchUrl`, which takes the user to Bing's Web search results page.
- Use the URL in `newsSearchUrl`, which takes the user to Bing's News search results page.
- Use the query text to call the [Web Search API](../../bing-web-search/overview.md) or [News Search API](../overview.md)yourself and display the results.

```json
{
  "_type": "TrendingTopics",
  "value": [
    {
      "webSearchUrl": "https://www.bing.com/search?q=Penguins+Australia+New+Zealand&form=TNSA01...",
      "name": "Not from Antarctic",
      "image": {
        "url": "https://www.bing.com/th?id=OPN.RTNews_Gce3fo6Et6sjm7kT0JYV-g&c=14&rs=...",
        "provider": [
          {
            "_type": "Organization",
            "name": "© David Merron Photography/Getty Images"
          }
        ]
      },
      "isBreakingNews": false,
      "query": {
        "text": "Penguins Australia New Zealand"
      },
      "newsSearchUrl": "https://www.bing.com/news/search?q=Penguins+Australia+New+Zealand&form=TNSA02..."
    },

    ...
  ]
}
```

[!INCLUDE [bing-url-note](../../../includes/bing-url-note.md)]

## Next steps

- Learn how to [get news by news category](category-news.md).
- Learn how to [get search for news](search-for-news.md) on the Web.
- Learn about [use and display requirements](../../bing-web-search/use-display-requirements.md) for Bing News Search.  
- Learn what happens if you don't stay within your queries per second (QPS) limit. Hint: your requests get [throttled](../../bing-web-search/throttling-requests.md).
