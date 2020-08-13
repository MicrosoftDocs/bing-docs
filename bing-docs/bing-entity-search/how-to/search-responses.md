---
title: "Handling a Bing Entity Search API response"
titleSuffix: Bing Search Services
description: The Bing Entity Search API returns entities and places. This topics shows you what the JSON response looks like and how to process it.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Handling the response 

When you send a request to Entity Search API, it returns a [SearchResponse](../reference/response-objects.md#searchresponse) object in the response body. The object may include one or more of the following answer types:

```json
{
    "_type": "SearchResponse",
    "queryContext": {...},
    "rankingResponse": {...},
    "entities": {...},
    "places": {...}
}
```

Basically, Bing returns any answer it finds that's relevant to the user's query, which is typically a subset of the possible answers. But Bing always returns the following answers in each response:

```json
{
    "_type": "SearchResponse",
    "queryContext": {...},
    "rankingResponse": {...}
}
```

But if an error occurs, the response body contains an [ErrorResponse](../reference/response-objects.md#errorresponse) object. Bing returns an error response for all 400 level HTTP status codes. [Read more](../reference/error-codes.md)

```json
{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidAuthorization", 
      "subCode": "AuthorizationMissing", 
      "message": "Authorization is required.", 
      "moreDetails": "Subscription key is not recognized."
    }
  ]
}
```

The rest of this topic provides details about each of the answer types in the **SearchResponse** object.

[!INCLUDE [bing-url-note](../../../includes/bing-url-note.md)]


## Entities answer

The [entities](../reference/response-objects.md#entityanswer) answer contains a list of entity objects that Bing thought were relevant to the query. Each [entity](../reference/response-objects.md#entity) in the list identifies a person, place, or thing. Bing returns well-known entities only. Well-known people may include singers, actors, athletes, models, and others. Places refers to well-known tourist attractions, organizations, and localities such as a cities, states, countries, and regions. Things cover everything else not covered by places and people, such as animals, foods, drinks, books, songs, movies, and more. For information about places such as restaurants, hotels, or other local businesses, see the [Places answer](#places-answer). 

### Dominant entity versus disambiguation entities

The list of entities may contain a single dominant entity, multiple disambiguation entities, or both. The following example fragments show the different entity types. See the `entityScenario` field.

**Dominant-only entity** (query is Seattle)

```json
  "entities": {
    "value": [
      {
        "name": "Seattle",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "City"
          ]
        },
      }
    ]
  },
```

**Dominant and disambiguation entities** (query is Mt Rainier)

```json
  "entities": {
    "value": [
      {
        "id": "https://<host>/api/v7/#Entities.0",
        "name": "Mount Rainier",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Place"
          ]
        },
      },
      {
        "id": "https://<host>/api/v7/#Entities.1",
        "name": "Mount Rainier National Park",
        "entityPresentationInfo": {
          "entityScenario": "DisambiguationItem"
        },
      }
    ]
  },
```

**Disambiguation-only entities** (query is Washington)

```json
  "entities": {
    "value": [
      {
        "id": "https://<host>/api/v7/#Entities.0",
        "name": "Washington, D.C.",
        "entityPresentationInfo": {
          "entityScenario": "DisambiguationItem",
          "entityTypeHints": [
            "City"
          ]
        },
      },
      {
        "id": "https://<host>/api/v7/#Entities.1",
        "name": "The Washington Post",
        "entityPresentationInfo": {
          "entityScenario": "DisambiguationItem",
          "entityTypeHints": [
            "Organization"
          ]
        },
      },
      {
        "id": "https://<host>/api/v7/#Entities.2",
        "name": "Washington",
        "entityPresentationInfo": {
          "entityScenario": "DisambiguationItem",
          "entityTypeHints": [
            "State"
          ]
        },
      }
    ]
  },
```

Bing returns a dominant entity when there is no ambiguity as to which entity satisfies the request. If multiple entities could satisfy the request, the list contains more than one disambiguation entities. For example, if the request uses the generic title of a movie franchise, the list likely contains disambiguation entities. But, if the request specifies a specific title from the franchise, the list likely contains a single dominant entity.

The [EntityPresentationInfo](../reference/response-objects.md#entitypresentationinfo) object contains information that tells you whether the entity is a dominant entity or a disambiguation entity (see the `entityScenario` field). The object may also include one or more hints (see the `entityTypeHints` field) that tell you the entity’s type. The list of hints could contain a single hint such as Movie or a list of hints such as Place, LocalBusiness, Restaurant. Each successive hint in the array narrows the entity's type. But not all entities include type hints.

If the list contains one or more disambiguation entities (the `entityScenario` field is set to DisambiguationItem), consider displaying a list of entities and letting the user select the one they’re interested in. The **Entity** object’s `name` field contains the entity’s name. Use the name along with the URL in the `url` field, if it exists, or the `webSearchUrl` field to create a hyperlink. The entity includes the `url` field only if Bing found a website or webpage for the entity. The URL in the `webSearchUrl` field takes the user to Bing’s search result page for the entity. 

The following answer shows what the JSON response looks like for the query, *mt rainier*. Most entities include the entity’s name, short description, contractual rules, and URL to Bing’s search results page where the user can get more information about the entity. The optional fields that not all entities include are the `image`, `url`, and `entityTypeHints` fields. 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "mt rainier"
  },
  "entities": {
    "value": [
      {
        "id": "https://<host>/api/v7/#Entities.0",
        "contractualRules": [
          {
            "_type": "ContractualRules/LicenseAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "license": {
              "name": "CC-BY-SA",
              "url": "http://creativecommons.org/licenses/by-sa/3.0/"
            },
            "licenseNotice": "Text under CC-BY-SA license"
          },
          {
            "_type": "ContractualRules/LinkAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "text": "Wikipedia",
            "url": "http://en.wikipedia.org/wiki/Mount_Rainier"
          },
          {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://en.wikipedia.org/wiki/Mount_Rainier"
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q=Mount+Rainier...",
        "name": "Mount Rainier",
        "image": {
          "name": "Mount Rainier",
          "thumbnailUrl": "https://www.bing.com/th?id=AMMS_65523b5b...",
          "provider": [
            {
              "_type": "Organization",
              "url": "http://en.wikipedia.org/wiki/Mount_Rainier"
            }
          ],
          "hostPageUrl": "http://upload.wikimedia.org/commons/Mount_Rainier...",
          "width": 110,
          "height": 110,
          "sourceWidth": 474,
          "sourceHeight": 316
        },
        "description": "Mount Rainier, also known as Tahoma or Tacoma, is a large active strato...",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Place"
          ]
        },
        "bingId": "9ae3e6ca-81ea-6fa1-ffa0-42e1d7890906"
      },
      {
        "id": "https://<host>/api/v7/#Entities.1",
        "contractualRules": [
          {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://en.wikipedia.org/wiki/Mount_Rainier_National_Park"
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q=Mount+Rainier+Nat...",
        "name": "Mount Rainier National Park",
        "url": "https://www.nps.gov/mora/index.htm",
        "image": {
          "name": "Mount Rainier National Park",
          "thumbnailUrl": "https://www.bing.com/th?id=AMMS_4bd2812676c04d54ef0e...",
          "provider": [
            {
              "_type": "Organization",
              "url": "http://en.wikipedia.org/wiki/Mount_Rainier_National_Park"
            }
          ],
          "hostPageUrl": "http://upload.wikimedia.org/Mount_Rainier_7437.JPG",
          "width": 72,
          "height": 72,
          "sourceWidth": 474,
          "sourceHeight": 355
        },
        "description": "Mount Rainier National Park is an American national park located in southeast...",
        "entityPresentationInfo": {
          "entityScenario": "DisambiguationItem"
        },
        "bingId": "9a8a1f72-a577-9f45-e275-1d969576f069"
      }
    ]
  },
```

### Entity attribution

Entities may include the `contractualRules` field, which contains one or more attributions that you must apply when you display the entity. Not all entities include rules. If the entity provides contractual rules, you must abide by them. 

Entity information typically comes from third parties. You are responsible for ensuring that your use is appropriate; for example, by complying with any creative commons license your user experience relies on.

For information about applying attribution, see [Data Attribution](../../bing-web-search/data-attribution.md).




## Places answer

The [places](../reference/response-objects.md#localentityanswer) answer contains a list of local business entity objects that Bing thought were relevant to the query. Bing returns this answer only when the query specifies the name of a local business or asks for a type of business. For example, *microsoft store* and *restaurants near me*. Each [place](../reference/response-objects.md#entity) in the list identifies a restaurant, hotel, or other local business.

> [!NOTE]
> The Places answer supports only U.S. business locations. 
 
> [!NOTE]
> You, or a third party on your behalf, may not use, retain, store, cache, share, or distribute any data from the Places answer for the purpose of testing, developing, training, distributing or making available any non-Microsoft service or feature.

Local aware queries such as *restaurant near me* require the user's location to provide accurate results. Although optional, you should always provide the user’s location using the X-Search-Location and X-MSEdge-ClientIP [headers](../reference/headers.md). The X-Search-Location header uses the user’s geographical coordinates (latitude and longitude). 

```
X-Search-Location: lat:47.806897;long:-122.221304;re:30
```

If you don’t provide the user’s location and Bing thinks the query would benefit from the user's location, it sets the `askUserForLocation` field of [QueryContext](../reference/response-objects.md#querycontext) to **true**.

```json
  "queryContext": {
    "originalQuery": "italian restaurants near me",
    "askUserForLocation": true
  },
```

The [EntityPresentationInfo](../reference/response-objects.md#entitypresentationinfo) object contains hints that identify the local entity's type. The list contains a list of hints such as Place, LocalBusiness, Restaurant. Each successive hint in the array narrows the entity's type. 

```json
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
```

The local entity includes the place's name, address, and telephone number. If the URL to the place’s website is known, the entity includes it, too. When you display the entity information, use the URL in the `url` field to create link that takes the user to the business' website; otherwise, use the URL in `webSearchUrl` to take the user to Bing’s search results page for the entity.

The following example shows what the JSON response looks like for the query, *coffee near me*. 

```json
  "places": {
    "value": [
      {
        "_type": "Restaurant",
        "id": "https://<host>/api/v7/#Places.0",
        "webSearchUrl": "https://www.bing.com/entityexplore?q=Fourth+Coffee...",
        "name": "Fourth Coffee",
        "url": "http://www.fourthcoffee.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Bothell",
          "addressRegion": "WA",
          "postalCode": "98021",
          "addressCountry": "US",
          "neighborhood": "Bothell"
        },
        "telephone": "(425) 555-1234"
      },

      . . .
    
    ]
  },
```

Note that the address’ `neighborhood` field may contain an empty string.

Note that the `_type` field identifies the local entity object's type. The above example shows the object's type as Restaurant. Others object types include Hotel and LocalBusiness.


## Next steps  

- Learn about [use and display requirements](../../bing-web-search/use-display-requirements.md) for displaying Bing Entity Search results.  
- Learn about how to use the `RankingResponse` object to [order the search results](../../bing-web-search/rank-results.md) in your UX.
- Learn about the [JSON objects](../reference/response-objects.md) found in the response.  
