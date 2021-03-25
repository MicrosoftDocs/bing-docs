---
title: "Search for videos using the Bing Video Search API"
titleSuffix: Bing Search Services
description: The Bing Video Search API finds and returns relevant videos from the web.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Search the web for videos

Use Bing Video Search API to search the Web for videos that matches the user's request.

It's easy. If you have your subscription key, just send an HTTP GET request to the following endpoint:

```
https://api.bing.microsoft.com/v7.0/videos/search
```

Here's a cURL example that shows you how to call the endpoint using your subscription key. Change the *q* query parameter to search for whatever videos you'd like.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/videos/search?q=sailing+dinghies
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
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -H "X-MSEdge-ClientID: 00B4230B74496E7A13CC2C1475056FF4" -H "X-MSEdge-ClientIP: 11.22.33.44" -H "X-Search-Location: lat:55;long:-111;re:22" -A "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.65 Safari/537.36" https://api.bing.microsoft.com/v7.0/videos/search?q=sailing+dinghies
```

Bing returns a couple of headers you should capture. 

- BingAPIs-TraceId &mdash; ID that identifies the request in the log file.
- X-MSEdge-ClientID &mdash; The ID that you need to pass in subsequent request to provide continuity of experience.
- BingAPIs-Market &mdash; The market used by Bing for the request.

To learn more about these headers, see [Response headers](../reference/headers.md#response-headers).

Here's a cURL call that returns the response headers. If you want to remove the response data so you can see only the headers, include the `-o nul` parameter.

```curl
curl -D - -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/videos/search?q=sailing+dinghies
```


## Query parameters

The only query parameter that you must pass is the *q* parameter, which you set to the user's query string. You must URL-encode the user's query string and all query parameter values that you pass.

The API supports a number of query parameters that you can pass in your request. Here's a list of the ones you're most likely to use.

- *count* and *offset* &mdash; Used to page image results. [Read more](../../bing-web-search/page-results.md)
- *mkt* &mdash; Used to specify the market where the results come from, which is typically the market where the user is making the request from.
- *safeSearch* &mdash; Used to specify the user's safe search preference.

To learn more about these parameters and other parameters that you may specify, see [Query parameters](../reference/query-parameters.md).

Here's a cURL example that includes these query parameters.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/videos/search?q=sailing+dinghies&mkt=en-us&safeSearch=moderate&count=10&offset=0
```

For information about query parameters that you can use to filter the search results, see [Filter the videos that Bing returns](#filter-the-videos-that-bing-returns).


## Filter the videos that Bing returns

When you query the Web, Bing returns all relevant videos that it finds. But what if you're only interested in videos that you can embed in your webpage or videos that are free? Simple, just use one or more of the following query parameters to filter the types of videos that you want. For more details about these parameters and others, see [Filter query parameters](../reference/query-parameters.md#filter-query-parameters).

- [aspect](../reference/query-parameters.md#aspect) &mdash; Filter videos by aspect ratio (for example, standard or wide screen).
- [embedded](../reference/query-parameters.md#embedded) &mdash; Filter for videos that you can embed in your webpage.
- [freshness](../reference/query-parameters.md#freshness) &mdash; Filter videos by age (for example, videos that Bing discovered in the past week).
- [pricing](../reference/query-parameters.md#pricing) &mdash; Filter by price (for example, free or paid).
- [resolution](../reference/query-parameters.md#resolution) &mdash; Filter videos by resolution (for example, videos that are 1080p or higher).
- [videoLength](../reference/query-parameters.md#videolength) &mdash; Filter videos by length (for example, videos that are between five and 20 minutes).


The following example shows how to get embeddable videos that Bing discovered in the past week that are free.  

```http
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/videos/search?q=sailing+dinghies&embedded=true&freshness=week&pricing=free&mkt=en-us 
```

### Filter images by website

To get videos from a specific domain, use the [site:](https://help.bing.microsoft.com/#apex/18/en-US/10001/-1) query operator in the query string. The response may contain results from other sites depending on the number of relevant results found on the specified site.

The following example gets sailing videos found on contososailing.com.

```http
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/videos/search?q=sailing+dinghies+site%3Acontososailing.com&mkt=en-us
```

> [!NOTE]
> Because the `site:` operator might include adult content regardless of the [safeSearch](../reference/query-parameters.md#safesearch) setting, only use `site:` if you're aware of the content on the domain.

### Filter by SafeSearch Setting

The [safeSearch](../reference/query-parameters.md#safesearch) query parameter lets you filter videos for adult content. 

You may set the *safeSearch* parameter to one of the following values:

- Moderate &mdash; Does not return videos with adult content. 
- Strict &mdash; Does not return videos with adult content.

The default is Moderate. If you specify Off, Bing ignores it and use Moderate.


## Next steps

- Learn about the [response](search-response.md) that Bing returns.
- Learn how to [get the next page](../../bing-web-search/page-results.md) of search results.
- Learn how to [get trending videos](trending-videos.md).
- Learn how to [get insights about a video](video-insights.md) such as related videos.
- Learn what happens if you don't stay within your queries per second (QPS) limit. Hint: your requests get [throttled](../../bing-web-search/throttling-requests.md).
- Learn about the [quickstarts](../quickstarts/quickstarts.md) and [samples](../samples.md) that are available to help you get up and running fast.

