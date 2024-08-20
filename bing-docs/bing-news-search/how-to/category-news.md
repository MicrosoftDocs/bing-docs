---
title: Get news by news categories
titleSuffix: Bing Search Services
description: Learn how to get news by news category.
services: bing-search-services
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: conceptual
author: alekhyasasi
ms.date: 07/07/2023
---

# Get news by news categories

If you’re building a news page and want to get headline news or category-specific news, such sports or world news, call News Category API.

To get headline news, call the API without including the [category](../reference/query-parameters.md#category) query parameter.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news?mkt=en-us
```

And if you want news for a specify news category, include the *category* parameter. The following example shows how to get sports news.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news?mkt=en-us&category=sports
```

For a list of possible news categories that you may specify, see [News categories by market](../reference/query-parameters.md#news-categories-by-market).

## Request and response headers

Bing suggests you include a couple of other headers to provide a better search experience for your user. Those headers include:

- User-Agent &mdash; Lets Bing know whether needs a mobile or desktop experience.
- X-MSEdge-ClientID &mdash; Provides continuity of experience.
- X-MSEdge-ClientIP &mdash; Provides the user's location for location aware queries.
- X-Search-Location &mdash; Provides the user's location for location aware queries.

The more information you can provide Bing, the better the search experience will be for your users. To learn more about these headers, see [Request headers](../reference/headers.md#request-headers).

Here's a cURL example that includes these headers.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -H "X-MSEdge-ClientID: 00B4230B74496E7A13CC2C1475056FF4" -H "X-MSEdge-ClientIP: 11.22.33.44" -H "X-Search-Location: lat:55;long:-111;re:22" -A "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.65 Safari/537.36" https://api.bing.microsoft.com/v7.0/news?mkt=en-us
```

Bing returns a couple of headers you should capture.

- BingAPIs-TraceId &mdash; ID that identifies the request in the log file.
- X-MSEdge-ClientID &mdash; The ID that you need to pass in subsequent request to provide continuity of experience.
- BingAPIs-Market &mdash; The market used by Bing for the request.

To learn more about these headers, see [Response headers](../reference/headers.md#response-headers).

Here's a cURL call that returns the response headers. If you want to remove the response data so you can see only the headers, include the `-o nul` parameter.

```curl
curl -D - -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news?mkt=en-us
```

## Query parameters

The API supports a number of query parameters that you can pass in your request. Here's a list of the ones you're most likely to use.

- *category* &mdash; Optional. Used to specify the news category to get news from. For a list of possible news categories that you may specify, see [News categories by market](../reference/query-parameters.md#news-categories-by-market). If not specified, the API returns up to 10 headline news articles.
- *mkt* &mdash; Used to specify the market where the results come from, which is typically the market where the user is making the request from. For a list of markets that this API supports, see [News Category API markets](../reference/market-codes.md#news-category-api-markets).

To learn more about these parameters, see [Query parameters](../reference/query-parameters.md).

Here's a cURL example that includes these query parameters.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news?mkt=en-us&category=sports
```

## What the response looks like

The response that News Category API returns is similar to what News Search API returns minus a few fields.

```json
{
  "_type": "News",
  "webSearchUrl": "https://www.bing.com/news/search?q=top+stories&form=TNSA02",
  "value": [
    {
      "name": "Democrats claim ‘big tent’ in first convention...",
      "url": "https://contosopress.com/fd1ec0b8c19b4d942972b6de5379857d",
      "image": {
        "thumbnail": {
          "contentUrl": "https://www.bing.com/th?id=ON.AA3F24A6607CF45B7A...",
          "width": 2420,
          "height": 1613
        },
        "isLicensed": false
      },
      "description": "Joe Biden and the Democrats are highlighting the party's inclusive...",
      "provider": [
        {
          "_type": "Organization",
          "name": "Contoso Press",
          "image": {
            "thumbnail": {
              "contentUrl": "https://www.bing.com/th?id=AMMS_6c39d1938749f17..."
            }
          }
        }
      ],
      "datePublished": "2020-08-14T14:22:00.0000000Z"
    },

    . . .
  ]
}
```

Notice that the [NewsAnswer](../reference/response-objects.md#newsanswer) object includes only the list of news articles (see the `value` field). And the [news articles](../reference/response-objects.md#newsarticle) don't include the `category` or `headline` fields.

[!INCLUDE [bing-url-note](../../../includes/bing-url-note.md)]

## Next steps

- Learn about the [response](search-response.md) that Bing returns.
- Learn how to [get trending news](trending-news.md).
- Learn how to [get search for news](search-for-news.md) on the Web.
- Learn about [use and display requirements](../../bing-web-search/use-display-requirements.md) for Bing News Search.  
- Learn what happens if you don't stay within your queries per second (QPS) limit. Hint: your requests get [throttled](../../bing-web-search/throttling-requests.md).
