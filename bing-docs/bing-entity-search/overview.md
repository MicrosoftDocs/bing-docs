---
title: What is the Bing Entity Search API?
titleSuffix: Bing Search Services
description: Use the Bing Entity Search API to extract and search for entities and places from search queries.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-entity-search
ms.topic: overview
ms.date: 07/15/2020
ms.author: scottwhi
---

# What is Bing Entity Search API?

Use Bing Entity Search API to get information about a well-known person, place (tourist attractions, states, countries/regions, etc.), or thing. Bing identifies the most relevant entity based on your searched term, spanning multiple entity types such as famous people, places, movies, TV shows, video games, books, and even local businesses near you. 

Local businesses include restaurants, hotel, or other local businesses. Bing returns a local business if the query specifies the name of the local business or asks for a type of business (for example, restaurants near me). 

|Feature  |Description  |
|---------|---------|
| [Entity disambiguation](how-to/search-for-entities.md#the-bing-entity-search-api-response)  | Get multiple entities for queries with multiple possible meanings. |
| [Find places](how-to/search-for-entities.md#find-places) | Search for and return information on local businesses and entities  |

## Workflow

The Bing Entity Search API is a RESTful web service, making it easy to call from any programming language that can make HTTP requests and parse JSON. You can use the service using either the REST API, or the SDK.

1. Create a [Cognitive Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) with access to the Bing Search APIs. If you don't have an Azure subscription, you can [create an account](https://azure.microsoft.com/free/cognitive-services/) for free.
2. Send a request to the API, with a valid search query.
3. Process the API response by parsing the returned JSON message.

## Next steps

* To get started quickly with your first request, try a [Quickstart](quickstarts/rest/csharp.md).
* The [Bing Entity Search API v7](reference/endpoints.md) reference.
* The [Bing Use and Display Requirements](../bing-web-search/use-display-requirements.md) specify acceptable uses of the content and information gained through the Bing search APIs.
