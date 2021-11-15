---
title: Search for news with the Bing News Search API
titleSuffix: Bing Search Services
description: Learn how to send search queries for general news, trending topics, and headlines.
services: bing-search-services
author: alekhyasasi
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 11/15/2021
ms.author: scottwhi
---

# Search the web for news

> [!NOTE]
> To comply with the new EU Copyright Directive in France, the Bing Web, News, Video, Image and all Custom Search APIs must omit some content from certain EU News sources for French users. The removed content may include thumbnail images and videos, video previews, and snippets which accompany search results from these sources. As a consequence, the Bing APIs may serve fewer results with thumbnail images and videos, video previews, and snippets to French users.

Use Bing News Search API to search the Web for news that's relevant to the user's search query.

It's easy. If you have your subscription key, just send an HTTP GET request to the following endpoint:

```
https://api.bing.microsoft.com/v7.0/news/search
```

Here's a cURL example that shows you how to call the endpoint using your subscription key. Change the *q* query parameter to search for whatever news you'd like.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news/search?q=mt+rainier
```


## Request and response headers

Although that's all you need to do to search the Web, Bing suggests you include a couple of other headers to provide a better search experience for your user. Those headers include:

- User-Agent &mdash; Lets Bing know whether the user needs a mobile or desktop experience.
- X-MSEdge-ClientID &mdash; Provides continuity of experience.
- X-MSEdge-ClientIP &mdash; Provides the user's location for location aware queries.
- X-Search-Location &mdash; Provides the user's location for location aware queries.

The more information you can provide Bing, the better the search experience will be for your users. To learn more about these headers, see [Request headers](../reference/headers.md#request-headers).

Here's a cURL example that includes these headers.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -H "X-MSEdge-ClientID: 00B4230B74496E7A13CC2C1475056FF4" -H "X-MSEdge-ClientIP: 11.22.33.44" -H "X-Search-Location: lat:55;long:-111;re:22" -A "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.65 Safari/537.36" https://api.bing.microsoft.com/v7.0/news/search?q=mt+rainier
```

Bing returns a couple of headers you should capture. 

- BingAPIs-TraceId &mdash; ID that identifies the request in the log file.
- X-MSEdge-ClientID &mdash; The ID that you need to pass in subsequent request to provide continuity of experience.
- BingAPIs-Market &mdash; The market used by Bing for the request.

To learn more about these headers, see [Response headers](../reference/headers.md#response-headers).

Here's a cURL call that returns the response headers. If you want to remove the response data so you can see only the headers, include the `-o nul` parameter.

```curl
curl -D - -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news/search?q=mt+rainier
```


## Query parameters

The only query parameter that you must pass is the *q* parameter, which you set to the user's query string. You must URL encode the user's query string and all query parameter values that you pass.

The API supports a number of query parameters that you can pass in your request. Here's a list of the ones you're most likely to use:

- *count* and *offset* &mdash; Used to page image results. [Read more](../../bing-web-search/page-results.md).
- *mkt* &mdash; Used to specify the market where the results come from, which is typically the market where the user is making the request from.
- *safeSearch* &mdash; Used to specify the user's safe search preference.
- *textDecorations* and *textFormat* &mdash; Used to turn on hit highlighting. [Read more](../../bing-web-search/hit-highlighting.md).

To learn more about these parameters and other parameters that you may specify, see [Query parameters](../reference/query-parameters.md).

Here's a cURL example that includes these query parameters.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news/search?q=mt+rainier&mkt=en-us&safeSearch=moderate&textdecorations=true&textformat=raw&count=10&offset=0
```

### Control the freshness of the articles

If you want to limit the age of the articles are that Bing returns, check out the [freshness](../reference/query-parameters.md) query parameter. For example, you can request that Bing return only articles that it discovered within the last day, week, or month. The [NewsArticle](../reference/response-objects.md#newsarticle) object's `datePublished` field contains the date that Bing discovered the article.


### Get news from a specific site

To get news from a specific domain, add the [site](https://help.bing.microsoft.com/#apex/18/en-US/10001/-1) query operator to the user's query string.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news/search?q=sailing+dinghies+site%3Acontososailing.com&mkt=en-us
```

### Get today's top news

To get today's top news articles, simply leave the `q` query parameter unset.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news/search?q=&mkt=en-us
```

The number of top news articles may vary depending on the news cycle.

Because there's a set number of top news results, the [NewsAnswer](../reference/response-objects.md#newsanswer) object doesn't include the `totalEstimatedMatches` field (you cannot page top news).

The **NewsAnswer** also doesn't include the `sort` field.

### Sorting the news articles

By default, the API orders the list of news articles by relevance with the most relevant articles listed first. If you want to order the articles by most recent, include the *sortBy* query parameter in your request and set it to date.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news/search?q=sailing&mkt=en-us&sortBy=date
```

The response’s `sort` field lists the sort options and indicates which option the request used (see the `isSelected` field). In the following example, the request used relevance to sort the articles. 
 
```json
  "sort": [
    {
      "name": "Best match",
      "id": "relevance",
      "isSelected": true,
      "url": "https://<host>/api/v7/news/search?q=sailing"
    },
    {
      "name": "Most recent",
      "id": "date",
      "isSelected": false,
      "url": "https://<host>/api/v7/news/search?q=sailing&sortby=date"
    }
  ],
```

### Including the original image

If the article includes an image, the image is a thumbnail of the article’s original image.

```json
      "image": {
        "thumbnail": {
          "contentUrl": "https://www.bing.com/th?id=ON.3EF9ED2DA5E74D39395D0E21...",
          "width": 700,
          "height": 525
        }
      },
```

To include the original image in the response, in addition to the thumbnail, include the *originalImg* query parameter and set it to true (the default is false).


```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/news/search?q=sailing&mkt=en-us&originalImg=true
```

Now the response includes both the `contentUrl` (link to the original image) and `thumbnail` fields.

```json
      "image": {
        "contentUrl": "https://www.windowscentral.com/sites/wpcentral.com/files...",
        "thumbnail": {
          "contentUrl": "https://www.bing.com/th?id=ON.3EF9ED2DA5E74D39395D0E21...",
          "width": 700,
          "height": 525
        }
      },
 ```


## Next steps

- Learn about the [response](search-response.md) that Bing returns.
- Learn how to [get the next page](../../bing-web-search/page-results.md) of search results.
- Learn how to [get trending news](trending-news.md).
- Learn how to [get news by category](category-news.md) such as sports or world.
- Learn what happens if you don't stay within your queries per second (QPS) limit. Hint: your requests get [throttled](../../bing-web-search/throttling-requests.md).
- Learn about the [quickstarts](../quickstarts/quickstarts.md) and [samples](../samples.md) that are available to help you get up and running fast.

