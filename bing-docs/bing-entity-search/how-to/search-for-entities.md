---
title: Search for entities with the Bing Entity Search API
titleSuffix: Bing Search Services
description: Bing Entity Search API returns entities and local business entities.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 06/26/2023
---

# Search the web for entities

Bing Entity Search API returns entities and local business entities.

Entities include people, places, or things. The API returns entity information for well-known entities only. Well-known people may include singers, actors, athletes, models, and others. *Places* refers to well-known tourist attractions, organizations, and localities such as a cities, states, countries, and regions. *Things* cover everything else not covered by places and people, such as animals, foods, drinks, books, songs, movies, and more.

Local business entities include restaurants, hotels, or other local businesses. The local business entities only when the query specifies the name of a local business or asks for a type of business. For example, *microsoft store* and *restaurants near me*.

> [!NOTE]
> The API supports only U.S. businesses for local business entities.

## The request

Making a request is easy if you have your subscription key. Just send an HTTP GET request to the following endpoint:

> <https://api.bing.microsoft.com/v7.0/entities>

Here's a cURL example that shows you how to call the endpoint using your subscription key. Change the *q* query parameter to search for whatever you'd like.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/entities?q=mt+rainier
```

## Request and response headers

Although that's all the more you need to do to search the web, Bing does suggest you include a couple of other headers to provide a better search experience for your user. Those headers include:

- User-Agent &mdash; Lets Bing know whether the user needs a mobile or desktop experience.
- X-MSEdge-ClientID &mdash; Provides continuity of experience.
- X-MSEdge-ClientIP &mdash; Provides the user's location for location aware queries.
- X-Search-Location &mdash; Provides the user's location for location aware queries.

The more information you can provide Bing, the better the search experience will be for your users. To learn more about these headers, see [Request headers](../reference/headers.md#request-headers).

Here's a cURL example that includes these headers.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -H "X-MSEdge-ClientID: 00B4230B74496E7A13CC2C1475056FF4" -H "X-MSEdge-ClientIP: 11.22.33.44" -H "X-Search-Location: lat:55;long:-111;re:22" -A "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.65 Safari/537.36" https://api.bing.microsoft.com/v7.0/entities?q=mt+rainier
```

Bing returns a couple of headers you should capture.

- BingAPIs-TraceId &mdash; ID that identifies the request in the log file.
- X-MSEdge-ClientID &mdash; The ID that you need to pass in subsequent request to provide continuity of experience.
- BingAPIs-Market &mdash; The market used by Bing for the request.

To learn more about these headers, see [Response headers](../reference/headers.md#response-headers).

Here's a cURL call that returns the response headers. If you want to remove the response data so you can see only the headers, include the `-o nul` parameter.

```curl
curl -D - -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/entities?q=mt+rainier
```

## Query parameters

The only query parameter that you must pass is the *q* parameter, which you set to the user's query string. You must URL encode the user's query string and all query parameter values that you pass.

The API supports a number of query parameters that you can pass in your request. Here's a list of the ones you're most likely to pass.

- *mkt* &mdash; Used to specify the market where the results come from, which is typically the market where the user is making the request from.
- *safeSearch* &mdash; Used to specify the user's safe search preference.
- *responseFilter* &mdash; Used to limit the results to a specific answer. For example, if you want only entities or only local business entities.

To learn more about these parameters and other parameters that you may specify, see [Query parameters](../reference/query-parameters.md).

Here's a cURL example that includes these query parameters.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/entities?q=mt_rainier&mkt=en-us&safeSearch=moderate&responseFilter=entities
```

## Next steps

- Learn about the [response](search-responses.md) that Bing returns for the user's query.
