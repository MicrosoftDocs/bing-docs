---
title: Web search endpoint
titleSuffix: Bing Search Services
description: To get web search results, send a `GET` request to the following endpoint. The headers and URL parameters define further specifications.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Web Search endpoint

The **Web Search API** returns Web pages, news, images, videos, and entities.

## Endpoint

To get Web search results using the Bing API, send a `GET` request to the following endpoint. The headers and URL parameters define further specifications.

**Endpoint**: Returns Web results that are relevant to the user's search query defined by `?q=""`.

```http
GET https://api.bing.microsoft.com/bing/v7.0/search
```

Endpoint: For details about headers, parameters, market codes, response objects, errors, and more, see the [Bing Web API v7](reference/endpoints.md) reference.

## Response JSON

The response to a Web search request includes all results as JSON objects. Parsing the result requires procedures that handle the elements of each type. See the [tutorial](tutorial/bing-web-search-single-page-app.md) and [source code](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/Tutorials/Bing-Web-Search) for examples.

## Next steps

The **Bing** APIs support search actions that return results according to their type. All search endpoints return results as JSON response objects.  All endpoints support queries that return a specific language and location by longitude, latitude, and search radius.

For complete information about the parameters supported by each endpoint, see the reference pages for each type.
For examples of basic requests using the Web search API, see [Search the Web Quick-starts](quickstarts/csharp.md).
