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

> [!NOTE]
> To comply with the new EU Copyright Directive in France, the Bing Web, News, Video, Image and all Custom Search APIs must omit some content from certain EU News sources for French users. The removed content may include thumbnail images and videos, video previews, and snippets which accompany search results from these sources. As a consequence, the Bing APIs may serve fewer results with thumbnail images and videos, video previews, and snippets to French users.


## Get started

To get started using the API, pick the subscription you want from <a href="https://aka.ms/bingsearchapipricing" target="_blank">Bing API Pricing</a>. After getting your subscription key, you're all set to make your first call. 

You can easily call the API by sending a native HTTP GET request or by using the Web Search SDK. For examples to help you get up and running quickly for either option, see [Quickstarts](quickstarts/quickstarts.md).


## Features  

By default, the API returns and ranks whatever content is relevant to the user's search query. But if you want to have some control over what Bing returns, see the following features:

|Feature|Description
|-|-
|[Filter the answers that bing returns](filter-answers.md)|Filter the response to include or exclude specific answers such as news or images, return webpages that Bing discovered within the last week, and more.
|[Page results](page-results.md)|Page through multiple pages of webpage results.
|[Hit highlighting](hit-highlighting.md)|Add highlighting characters to words and phrases in the results' titles and descriptions that identify the words or phrases from the user's search query.

Bing also provides API metrics, which you can use to inform your strategic decisions. Quickly retrieve statistics such as your top queries, call volume, market distribution, response code summary, and many more. For details, see [Bing Web Statistics](bing-web-stats.md).


### Search or search-like experience

Bing Web Search API may only be used as a result of a direct user query or search, or as a result of an action within an app or experience that logically can be interpreted as a user’s search request. For illustration purposes, the following are some examples of acceptable search or search-like experiences.

- User enters a query directly into a search box in an app
- User selects specific text or image and requests “more information” or “additional information”
- User asks a search bot about a particular topic
- User dwells on a particular object or entity in a visual search type scenario

If you are not sure if your experience can be considered a search-like experience, check with Microsoft.


## Next steps

- Learn about other APIs in the [family of Bing Search APIs](bing-api-comparison.md).
- Learn about [use and display requirements](use-display-requirements.md) for Bing Web Search.  
- Learn about [calling the API](search-the-web.md).
- Learn about what's in the [JSON response](search-responses.md).
- Review [Web Search API v7 reference](reference/endpoints.md) documentation.  
