---
title: Search the web
titleSuffix: Bing Search Services
description: Use Bing Web Search API to surface relevant information from billions of web documents.
services: bing-search-services
author:  swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---
> [!WARNING] 
> **Product to be retired** Bing Search and Bing Custom Search APIs will be retired on 11th August 2025. 
> New deployments are not available and existing resources will be disabled. [Learn more](https://aka.ms/BingAPIsRetirement)
<br/>

# Search the web

Use Bing Web Search API to search billions of web documents for content that's relevant to the user's search string.

It's easy. If you have your subscription key, just send an HTTP GET request to the following endpoint:

```
https://api.bing.microsoft.com/v7.0/search
```

Here's a cURL example that shows you how to call the endpoint using your subscription key. Change the *q* query parameter to search for whatever you'd like.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/search?q=microsoft+devices
```

## Request and response headers

Although that's all you need to do to search the web, Bing suggests you include a couple of other headers to provide a better search experience for your user. Those headers include:

- User-Agent &mdash; Lets Bing know whether needs a mobile or desktop experience.
- X-MSEdge-ClientID &mdash; Provides continuity of experience.
- X-MSEdge-ClientIP &mdash; Provides the user's location for location aware queries.
- X-Search-Location &mdash; Provides the user's location for location aware queries.

The more information you can provide Bing, the better the search experience will be for your users. To learn more about these headers, see [Request headers](reference/headers.md#request-headers).

Here's a cURL example that includes these headers.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -H "X-MSEdge-ClientID: 00B4230B74496E7A13CC2C1475056FF4" -H "X-MSEdge-ClientIP: 11.22.33.44" -H "X-Search-Location: lat:55;long:-111;re:22" -A "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.65 Safari/537.36" https://api.bing.microsoft.com/v7.0/search?q=microsoft+devices
```

Bing returns a few headers you should capture.

- BingAPIs-TraceId &mdash; The ID that identifies the request in the log file.
- X-MSEdge-ClientID &mdash; The ID that you need to pass in subsequent request to provide continuity of experience.
- BingAPIs-Market &mdash; The market used by Bing for the request.

To learn more about these headers, see [Response headers](reference/headers.md#response-headers).

Here's a cURL call that returns the response headers. If you want to remove the response data so you can see only the headers, include the `-o nul` parameter.

```curl
curl -D - -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/search?q=microsoft+devices
```

## Query parameters

The only query parameter that you must pass is the *q* parameter, which you set to the user's query string. You must URL-encode the user's query string and all query parameter values that you pass.

The API supports a number of query parameters that you can pass in your request. Here's a list of the ones you're most likely to pass.

- *count* and *offset* &mdash; Used to page webpage results. [Read more](page-results.md)
- *mkt* &mdash; Used to specify the market where the results come from, which is typically the market where the user is making the request from.
- *safeSearch* &mdash; Used to specify the user's safe search preference.
- *textDecorations* and *textFormat* &mdash; Used to turn on hit highlighting. [Read more](hit-highlighting.md)

To learn more about these parameters and other parameters that you may specify, see [Query parameters](reference/query-parameters.md).

Here's a cURL example that includes these query parameters.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/search?q=microsoft+devices&mkt=en-us&safeSearch=moderate&textdecorations=true&textformat=raw&count=10&offset=0
```

For information about query parameters that you can use to filter the search results, see [Filtering search results](filter-answers.md).

## Next steps

- Learn about the [response](search-responses.md) that Bing returns for the user's query.
- Learn how to [get the next page](page-results.md) of search results.
- Learn what happens if you don't stay within your queries per second (QPS) limit. Hint: your requests get [throttled](throttling-requests.md).
- Learn about the [quickstarts](quickstarts/quickstarts.md) and [samples](samples.md) that are available to help you get up and running fast.
