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

The Bing News Search API makes it easy to integrate Bing's cognitive news searching capabilities into your applications. The API provides a similar experience to [Bing News](https://www.bing.com/news), letting you send search queries and receive relevant news articles.

Be aware that the Bing News Search API provides news search results only. Use the [Bing Web Search API](../bing-web-search/overview.md), [Video Search API](../bing-video-search/overview.md) and [Image Search API](../bing-image-search/overview.md) for other types of web content.

## Bing News Search API features

While the Bing News Search API primarily finds and returns relevant news articles, it provides several features for intelligent, and focused news retrieval on the web.

|Feature  |Description  |
|---------|---------|
|[Get general news](concepts/search-for-news.md#get-general-news)     | Find news by sending a search query to the Bing News Search API, and getting back a list of relevant news articles.           |
|[Today's top news](concepts/search-for-news.md#get-todays-top-news)      | Get the top news stories for the day, across all categories.       |
|[News by category](concepts/search-for-news.md)     | Search for news in specific categories.        | 
|[Headline news](concepts/search-for-news.md)     | Search for top headlines across all categories.         |

## Workflow

The Bing News Search API is a RESTful web service, making it easy to call from any programming language that can make HTTP requests and parse JSON. You can use the service using either the REST API, or the SDK.

1. Create a [Cognitive Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) with access to the Bing Search APIs. If you don't have an Azure subscription, you can [create an account](https://azure.microsoft.com/free/cognitive-services/) for free.
2. Send a request to the API, with a valid search query.
3. Process the API response by parsing the returned JSON message.

## Next steps

To quickly get started with your first API request, try a quickstart for the [REST API](quickstarts/csharp.md) or one of the [SDKs](quickstarts/client-libraries.md).

## See also

* The [Bing News Search API v7](reference/endpoints.md) reference section contains definitions and information on the endpoints, headers, API responses, and query parameters that you can use to request image-based search results.
* The [Bing Use and Display Requirements](../bing-web-search/use-display-requirements.md) specify acceptable uses of the content and information gained through the Bing search APIs.