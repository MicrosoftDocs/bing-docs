---
title: What is Bing Autosuggest?
titleSuffix: Bing Search Services
description: The Bing Autosuggest API returns a list of suggested queries based on the partial query string in the search box.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 07/15/2020
ms.author: scottwhi
---

# What is Bing Autosuggest?

Use Bing Autosuggest API to improve your users' search box experience by providing a list of suggested queries with each character they type.

As the user types their search query, send Bing the partial query string and get back suggestions. The more complete the user’s query string is, the more relevant the list of suggested query terms will be. For example, the suggestions that Bing might return for *m* are likely to be less relevant than the suggestions Bing returns for *micro*. 

The suggestions are based on user intent and past searches made by the user and others.

## Bing Autosuggest API features

|Feature|Description
|-|-
|[Suggest search terms in real-time](how-to/get-suggestions.md)|Improve your app experience by using the Autosuggest API to display suggested search terms as they're typed.

## Workflow

The Bing Autosuggest API is a RESTful web service, easy to call from any programming language that can make HTTP requests and parse JSON.

1. Create a [Cognitive Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) with access to the Bing Search APIs. If you don't have an Azure subscription, you can [create an account](https://azure.microsoft.com/free/cognitive-services/) for free.
2. Send a request to this API each time a user types a new character in your application's search box.
3. Process the API response by parsing the returned JSON message.

Typically, you'd call this API each time the user types a new character in your application's search box. As more characters are entered, the API will return more relevant suggested search queries. For example, the suggestions the API might return for a single `s` are likely to be less relevant than ones for `sail`.

The following example shows a drop-down search box with suggested query terms from the Bing Autosuggest API.

![Autosuggest drop-down search box list](media/bing-autosuggest-drop-down-list.PNG)

When a user selects a suggestion from the drop-down list, you can use it to begin searching with one of the Bing Search APIs, or directly go to the Bing search results page.

## Next steps

To get started quickly with your first request, see [Making Your First Query](quickstarts/rest/csharp.md).

Familiarize yourself with the [Bing Autosuggest API v7](reference/endpoints.md) reference. The reference contains the list of endpoints, headers, and query parameters that you'd use to request suggested query terms, and the definitions of the response objects.

Learn how to search the web by using the [Bing Web Search API](../bing-web-search/overview.md) and explore the other [Bing Search APIs](../bing-web-search/bing-api-comparison.md).

Be sure to read [Bing Use and Display Requirements](../bing-web-search/use-display-requirements.md) so you don't break any of the rules about using the search results.
