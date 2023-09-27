---
title: What is the Bing Entity Search API?
titleSuffix: Bing Search Services
description: Use Bing Entity Search API to search for entities and places.
services: bing-search-services
author: alekhyasasi
ms.author: v-alpunnamar
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-entity-search
ms.topic: overview
ms.date: 09/26/2023
---

# What is Bing Entity Search API?

Bing Entity Search API returns entities and local business entities.

Entities include people, places, or things. The API returns entity information for well-known entities only. Well-known people may include singers, actors, athletes, models, and others. *Places* refers to well-known tourist attractions, organizations, and localities such as a cities, states, countries, and regions. *Things* cover everything else not covered by places and people, such as animals, foods, drinks, books, songs, movies, and more.

Local business entities include restaurants, hotels, or other local businesses. The local business entities only when the query specifies the name of a local business or asks for a type of business. For example, *microsoft store* and *restaurants near me*.

> [!NOTE]
>
> - The API supports only U.S. businesses for local business entities.
> - You, or a third party on your behalf, may not use, retain, store, cache, share, or distribute any data from the Places answer for the purpose of testing, developing, training, distributing or making available any non-Microsoft service or feature.

## Get started

To get started using the API, pick the subscription you want from <a href="https://aka.ms/bingsearchapipricing" target="_blank">Bing API Pricing</a>. After getting your subscription key, you're all set to make your first call.

You can easily call the API by sending a native HTTP GET request or by using the Entity Search SDK.

## View API metrics

Bing provides API metrics, which you can use to inform your strategic decisions. Quickly retrieve statistics such as your top queries, call volume, market distribution, response code summary, and many more. For details, see [Bing Web Statistics](../bing-web-search/bing-web-stats.md).

### Search or search-like experience

Bing Entity Search API may only be used as a result of a direct user query or search, or as a result of an action within an app or experience that logically can be interpreted as a user’s search request. For illustration purposes, the following are some examples of acceptable search or search-like experiences.

- User enters a query directly into a search box in an app.
- User selects specific text or image and requests “more information” or “additional information”.
- User asks a search bot about a particular topic.
- User dwells on a particular object or entity in a visual search type scenario.

If you are not sure if your experience can be considered a search-like experience, check with Microsoft.

## Next steps

- Learn about other APIs in the [family of Bing Search APIs](../bing-web-search/bing-api-comparison.md).
- Learn about [use and display requirements](../bing-web-search/use-display-requirements.md) for Bing Web Search.  
- Learn about [calling the API](how-to/search-for-entities.md).
- Review [Entity Search API v7 reference](reference/endpoints.md) documentation.  
