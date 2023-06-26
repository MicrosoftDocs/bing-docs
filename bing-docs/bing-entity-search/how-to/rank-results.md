---
title: Using ranking to display answers - Bing Entity Search
titleSuffix: Bing Search Services
description: Learn how to use ranking to display the answers that the Bing Entity Search API returns.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 06/26/2023
---

# Use ranking to display entity search results  

Each entity search response includes a [RankingResponse](../reference/response-objects.md#rankingresponse) answer that tells you how to display the search results. For details about using the ranking response, see [Web Search API](../../bing-web-search/rank-results.md).

## Ranking response examples

The following shows an example **RankingResponse** object for entities.
  
```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Jimi Hendrix"
  },
  "entities": { ... },
  "rankingResponse": {
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        },
        {
          "answerType": "Entities",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.1"
          }
        }
      ]
    }
  }
}
```

Based on this ranking response, you'd display both entities in the sidebar.

And this example shows what the ranking response looks like for local business entities.

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "hilton near me",
    "askUserForLocation": true
  },
  "places": { ... },
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "Places"
        },
        {
          "answerType": "Places",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Places.0"
          }
        },
        {
          "answerType": "Places",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Places.1"
          }
        },
        {
          "answerType": "Places",
          "resultIndex": 2,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Places.2"
          }
        },
        {
          "answerType": "Places",
          "resultIndex": 3,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Places.3"
          }
        },
        {
          "answerType": "Places",
          "resultIndex": 4,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Places.4"
          }
        }
      ]
    }
  }
}
```

## Next steps

- Learn about [sending entity search requests](search-for-entities.md).
