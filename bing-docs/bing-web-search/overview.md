---
title: What is the Bing Web Search API?
titleSuffix: Bing Search Services
description: Bing Web Search API enables safe, ad-free, location-aware search results, surfacing relevant information from billions of web documents.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: overview
ms.date: 07/15/2020
ms.author: scottwhi
---

# What is the Bing Web Search API?

Bing Web Search API enables safe, ad-free, location-aware search results, surfacing relevant information from billions of web documents. Help your users find what they're looking for from the world-wide-web by harnessing Bing's ability to comb billions of webpages, images, videos, and news with a single API call.


## Get started

To get started using the API, pick the subscription you want from <a href="https://www.microsoft.com/en-us/bing/apis/pricing" target="_blank">Bing API Pricing</a>. After getting your subscription key, you're all set to make your first call. 

You can easily call the API by sending a native HTTP GET request or by using the Web Search SDK. For examples to help you get up and running quickly for either option, see the [REST quickstart example](quickstarts/rest/csharp.md) or [SDK quickstart example](quickstarts/sdk/web-search-client-library-csharp.md).


## Features  

By default, the API returns and ranks whatever content is relevant to the user's search query. But if you want to have some control over what Bing returns, see the following features:

|Feature|Description
|-|-
|[Filter the answers that bing returns](filter-answers.md)|Filter the response to include or exclude specific answers such as news or images, return webpages that Bing discovered within the last week, and more.
|[Page results](page-results.md)|Page through multiple pages webpage results.
|[Hit highlighting](hit-highlighting.md)|Add highlighting characters to words and phrases in the results' titles and descriptions that identify the words or phrases from the user's search query.

Bing also provides API metrics, which you can use to inform your strategic decisions. Quickly retrieve statistics such as your top queries, call volume, market distribution, response code summary, and many more. For details, see [Bing Web Statistics](bing-web-stats.md).


## Next steps

- Learn about other APIs in the [family of Bing Search APIs](bing-api-comparison.md).
- Learn about [use and display requirements](use-display-requirements.md) for Bing Web Search.  
- Learn about [calling the API](search-the-web.md).
- Learn about what's in the [JSON response](search-responses.md).
- Review [Web Search API v7 reference](reference/endpoints.md) documentation.  
