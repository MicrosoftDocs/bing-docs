---
title: How to use rankings to display search results
titleSuffix: Bing Search Services
description: Learn how to use ranking to display search results from the Bing Web Search API.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Use ranking to display search results  

Each search response includes a [RankingResponse](reference/response-objects.md#rankingresponse) answer tells you how to display the search results. The ranking response groups results by mainline content and sidebar content for a traditional search results page. If you don't display the results in a traditional mainline and sidebar format, you must provide the mainline content higher visibility than the sidebar content.  

The following example fragment shows what the **RankingResponse** answer looks like in the JSON response for the query, *how to use saffron threads*.

```json
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "Videos",
          "value": {
            "id": "https://<host>/api/v7.0/#Videos"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 0,
          "value": {
            "id": "https://<host>/api/v7.0/#WebPages.0"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 1,
          "value": {
            "id": "https://<host>/api/v7.0/#WebPages.1"
          }
        },

        . . .

        {
          "answerType": "WebPages",
          "resultIndex": 9,
          "value": {
            "id": "https://<host>/api/v7.0/#WebPages.9"
          }
        },
        {
          "answerType": "RelatedSearches",
          "value": {
            "id": "https://<host>/api/v7.0/#RelatedSearches"
          }
        }
      ]
    },
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "<host>/api/v7.0/#Entities.0"
          }
        }
      ]
    }
  }
```

Within each group (mainline or sidebar), the [Items](reference/response-objects.md#rankinggroup-items) array identifies the order that the content must appear in. Each item provides the following two ways to identify the result within an answer.  

- `answerType` and `resultIndex`  
  
  The `answerType` field identifies the answer (for example, the News answer) and the `resultIndex` field identifies a result within the answer (for example, a news article). The result index is zero based.  
  
- `value`  
  
  The `value` field contains an ID that matches the ID of either an answer or a result within the answer. Either the answer or the results contain the ID but not both. The fragment portion of the URI identifies the answer type and index. For example, `https://<host>/api/v7.0/#WebPages.9`, identifies the 10th webpage in the Webpages answer. 


Because the videos ranking item doesn’t include the `resultIndex` field and the `id` URI is missing the index value, you’d display all video results together. 

```json
        {
          "answerType": "Videos",
          "value": {
            "id": "https://<host>/api/v7.0/#Videos"
          }
        },
```

The following JSON shows the videos answer in the response. Notice that the Videos answer object includes the id field and the Video result objects don’t. Either the answer will contain the id field or the results will, but not both.

```json
  "videos": {
    "id": "https://<host>/api/v7.0/#Videos",
    "readLink": "https://<host>/api/v7.0/videos/search?q=how+to+use+saffron+threads",
    "webSearchUrl": "https://www.bing.com/videos/search?q=how+to+use+saffron+threads",
    "isFamilyFriendly": true,
    "value": [
      {
        "webSearchUrl": "https://www.bing.com/videos/search?q=how%20to%20use%20sa...",
        "name": "How to use Saffron Threads",
        "description": "How to use Saffron Threads. Best way to use saffron threads for maximum flavour and color.",
        "thumbnailUrl": "https://tse3.mm.bing.net/th?id=OVP.qj4aGA...",
        "datePublished": "2018-08-21T17:01:27.0000000",
        "publisher": [
          {
            "name": "Contoso"
          }
        ],
        "isAccessibleForFree": true,
        "contentUrl": "https://www.contoso.com/watch?v=Ai7LksfYDPs",
        "hostPageUrl": "https://www.contoso.com/watch?v=Ai7LksfYDPs",
        "encodingFormat": "mp4",
        "hostPageDisplayUrl": "https://www.contoso.com/watch?v=Ai7LksfYDPs",
        "width": 1280,
        "height": 720,
        "duration": "PT45S",
        "motionThumbnailUrl": "https://tse3.mm.bing.net/th?id=OM1.F1163Ia21PXw5w_...",
        "embedHtml": "<iframe width=\"1280\" height=\"720\" src=\"http://www.youtube.com/embed/Ai7LksfYDPs?autoplay=1\" frameborder=\"0\" allowfullscreen></iframe>",
        "allowHttpsEmbed": true,
        "viewCount": 349,
        "thumbnail": {
          "width": 160,
          "height": 120
        },
        "allowMobileEmbed": true,
        "isSuperfresh": false
      },
```

However, because the Webpages answer doesn’t include an `id` field, you'd display all webpages individually based on the ranking (each webpage includes an `id` field). Either the answer will contain the `id` field or the results will, but not both.

```json
  "webPages": {
    "webSearchUrl": "https://www.bing.com/search?q=how+to+use+saffron+threads",
    "totalEstimatedMatches": 1020000,
    "value": [
      {
        "id": "https://<host>/api/v7.0/#WebPages.0",
        "name": "3 Ways to Prepare Saffron",
        "url": "https://www.fabrikam.com/Prepare-Saffron",
        "about": [
          {
            "name": "Saffron"
          }
        ],
        "isFamilyFriendly": true,
        "displayUrl": "https://www.fabrikam.com/Prepare-Saffron",
        "snippet": "Measure the saffron threads. Your recipe will usually tell you how much saffron to use...",
        "dateLastCrawled": "2020-02-18T22:19:00.0000000Z",
        "language": "en",
        "isNavigational": false
      },
```

To use the ranking ID, simply match the ranking ID with the ID of an answer or one of its results. If you use the `answerType` and `resultIndex` fields, use `answerType` to identify the answer that contains the results to display. Then, use `resultIndex` to index through the answer's results to get the result to display. 

Based on the ranking response example for the saffron query, you’d display the following search results in the mainline:

- All the videos (or several videos with a link to view the others)
- Webpages 0 through 9
- All the related searches

And you’d display the following search results in the sidebar:

- Entity 0


## Next steps

- Learn about [promoting unranked results](filter-answers.md#promoting-answers-that-are-not-ranked).
- Learn about [limiting the number of ranked answers in the response](filter-answers.md#returning-the-top-n-answers).
- See the [ranking to tutorial](tutorial/csharp-ranking-tutorial.md) to see how to display search results in C#.
