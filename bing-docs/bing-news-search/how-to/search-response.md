---
title: "Sending queries to the Bing News Search API"
titleSuffix: Bing Search Services
description: The Bing News Search API enables you to search the web for relevant news items. Use this article to learn more about sending search queries to the API.
services: bing-search-services
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: conceptual
author: alekhyasasi
ms.date: 07/07/2023
---

# Handling the news search response

When you send a request to News Search API, it returns a [NewsAnswer](../reference/response-objects.md#newsanswer) object in the response body. The object may include one or more of the following fields:

```json
{
  "_type": "News",
  "readLink": "https://api.bing.microsoft.com/api/v7/news/search?q=us+politics",
  "queryContext": { ... },
  "totalEstimatedMatches": 9890000,
  "sort": [ { ... } ],
  "value": [ { ... } ]
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

[!INCLUDE [bing-url-note](../../../includes/bing-url-note.md)]

The **NewsAnswer** object's `value` field contains a list of [NewsArticle](../reference/response-objects.md#newsarticle) objects. Here's what most of the news objects look like in the response.

```json
    {
      "name": "Hunger to break the old political system grows",
      "url": "https:\/\/www.contosonews.com\/politics\/politics-news...",
      "image": {
        "thumbnail": {
          "contentUrl": "https:\/\/www.bing.com\/th?id=ON.0430E0FD42944...",
          "width": 700,
          "height": 367
        }
      },
      "description": "As they kick off an all-virtual convention this week, Democrats aren't...",
      "provider": [
        {
          "_type": "Organization",
          "name": "Contoso News",
          "image": {
            "thumbnail": {
              "contentUrl": "https:\/\/www.bing.com\/th?id=AR_e4bde9ad3949725..."
            }
          }
        }
      ],
      "datePublished": "2020-08-15T15:55:00.0000000Z"
    },
```

Each news article includes the article's `name`, `description`, `image`, and `url` to the article on the host's website. Be sure to use `provider` to attribute the article.

If Bing can determine the category of news article, the article includes the `category` field.

```json
      "category": "Politics"
```

Some articles include the `video` field, which contains a video that's relevant to the news story. Bing provides an iFrame that you can embed in your HTML to play the video.

```json
      "video": {
        "name": "The Political Conventions Are Starting...",
        "thumbnailUrl": "https:\/\/www.bing.com\/th?id=ON.C34094BFB2230182BC...",
        "embedHtml": "<iframe title=\"Contoso News Video - Embed Player\" width=\"480\" height=\"321\" frameborder=\"0\" scrolling=\"no\" allowfullscreen=\"true\" marginheight=\"0\" marginwidth=\"0\" id=\"contoso_video_player\" src=\"https:\/\/www.contosonews.com\/video\/players\/offsite\/...\"><\/iframe>",
        "allowHttpsEmbed": true,
        "thumbnail": {
          "width": 75,
          "height": 75
        }
      },
```

Note that in some cases, the [Video](../reference/response-objects.md#video) object includes only a thumbnail and not the embedded video.

```json
      "video": {
        "name": "US politics: virtual Democratic convention to start amid coronavirus crisis – live updates",
        "thumbnailUrl": "https:\/\/www.bing.com\/th?id=ON.0C2BD59684791402EB8ED247AC9C4B58&pid=News",
        "thumbnail": {
          "width": 480,
          "height": 288
        }
      },
```

## Next steps

- Learn how to [get trending news](trending-news.md).
- Learn how to [get news by news category](category-news.md).
- Learn about [use and display requirements](../../bing-web-search/use-display-requirements.md) for Bing News Search.
