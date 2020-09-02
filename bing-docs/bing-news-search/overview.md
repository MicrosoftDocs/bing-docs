---
title: What is the Bing News Search API?
titleSuffix: Bing Search Services
description: Learn how to use the Bing News Search API to search the web for current headlines across categories, including headlines and trending topics.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: overview
ms.date: 07/15/2020
ms.author: scottwhi
---

# What is the Bing News Search API?

Bing News Search API lets your users find headline news, news by category, and trending articles. If you're looking to build an experience similar to [bing.com/news](https://www.bing.com/news), this is the API to use.

> [!NOTE]
> To comply with the new EU Copyright Directive in France, the Bing Web, News, Video, Image and all Custom Search APIs must omit some content from certain EU News sources for French users. The removed content may include thumbnail images and videos, video previews, and snippets which accompany search results from these sources. As a consequence, the Bing APIs may serve fewer results with thumbnail images and videos, video previews, and snippets to French users.


## Get started

To get started using the API, pick the subscription you want from <a href="https://www.microsoft.com/en-us/bing/apis/pricing" target="_blank">Bing API Pricing</a>. After getting your subscription key, you're all set to make your first call. 

You can easily call the API by sending a native HTTP GET request or by using the News Search SDK. For examples to help you get up and running quickly for either option, see [Quickstarts](quickstarts/quickstarts.md).



## Features

While Bing News Search API primarily focuses on finding relevant news articles, it also provides several features for intelligent and focused news retrieval on the web.

|Feature|Description
|-|-
|[Get general news](how-to/search-for-news.md)|Gets general news stories based on the user's search query. If the search query is empty, the API returns top news articles from different categories.
|[Today's trending news stories](how-to/trending-news.md)|Gets news stories that are trending on social networks.
|[News by category](how-to/category-news.md)|Search for news in specific news categories like business, sports, or entertainment.        | 

Bing also provides API metrics, which you can use to inform your strategic decisions. Quickly retrieve statistics such as your top queries, call volume, market distribution, response code summary, and many more. For details, see [Bing Web Statistics](../bing-web-search/bing-web-stats.md).


### Search or search-like experience

Bing News Search API may only be used as a result of a direct user query or search, or as a result of an action within an app or experience that logically can be interpreted as a user’s search request. For illustration purposes, the following are some examples of acceptable search or search-like experiences.

- User enters a query directly into a search box in an app
- User selects specific text or image and requests “more information” or “additional information”
- User asks a search bot about a particular topic
- User dwells on a particular object or entity in a visual search type scenario

If you are not sure if your experience can be considered a search-like experience, check with Microsoft.


## Next steps

- Learn about other APIs in the [family of Bing Search APIs](../bing-web-search/bing-api-comparison.md).
- Learn about [use and display requirements](../bing-web-search/use-display-requirements.md) for Bing Web Search.  
- Learn about [searching the web for news](how-to/search-for-news.md).
- Learn about what's in the [JSON response](how-to/search-response.md).
- Review [News Search API v7 reference](reference/endpoints.md) documentation.  
