---
title: The Bing Entity Search API endpoint
titleSuffix: Bing Search Services
description: The Bing Entity Search API has one endpoint that returns entities from the Web based on a query. These search results are returned in JSON.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Bing Entity Search API endpoint


The Bing Entity Search API has one endpoint that returns entities from the Web based on a query. These search results are returned in JSON.

## Get entity results from the endpoint

To get entity results using the **Bing API**, send a `GET` request to the following endpoint. Use [headers](reference/headers.md) and [query parameters](reference/query-parameters.md) to customize your search request. Search requests can be sent using the `?q=` parameter.

```cURL
 GET https://api.bing.microsoft.com/bing/v7.0/entities
```

## Next steps

> [!div class="nextstepaction"]
> [What is the Bing Entity Search API?](overview.md)

