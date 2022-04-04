---
title: Search the web for trending videos using the Bing Video Search API
titleSuffix: Bing Search Services
description: Learn how to use the Bing Video Search API to search the web for trending videos.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 04/04/2022
ms.author: v-apunnamara
---

# Get videos that are trending

If you’re building a user experience that shows videos that are trending on social media in different categories, use this API.

Calling the API is easy. If you have your subscription key, just send an HTTP GET request to the following endpoint:

```curl
https://api.bing.microsoft.com/v7.0/videos/trending
```

Here's a cURL example that shows you how to call the endpoint using your subscription key.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/videos/trending
```  

## Request and response headers

Although that's all you need to do to get trending videos, Bing suggests you include a couple of other headers to provide a better experience for your user. Those headers include:

- User-Agent &mdash; Lets Bing know whether the user needs a mobile or desktop experience.
- X-MSEdge-ClientID &mdash; Provides continuity of experience.
- X-MSEdge-ClientIP &mdash; Provides the user's location for location aware queries.
- X-Search-Location &mdash; Provides the user's location for location aware queries.

The more information you can provide Bing, the better the experience will be for your users. To learn more about these headers, see [Request headers](../reference/headers.md#request-headers).

Here's a cURL example that includes these headers.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -H "X-MSEdge-ClientID: 00B4230B74496E7A13CC2C1475056FF4" -H "X-MSEdge-ClientIP: 11.22.33.44" -H "X-Search-Location: lat:55;long:-111;re:22" -A "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.65 Safari/537.36" https://api.bing.microsoft.com/v7.0/videos/trending
```

Bing returns a couple of headers you should capture.

- BingAPIs-TraceId &mdash; ID that identifies the request in the log file.
- X-MSEdge-ClientID &mdash; The ID that you need to pass in subsequent request to provide continuity of experience.
- BingAPIs-Market &mdash; The market used by Bing for the request.

To learn more about these headers, see [Response headers](../reference/headers.md#response-headers).

Here's a cURL call that returns the response headers. If you want to remove the response data so you can see only the headers, include the `-o nul` parameter.

```curl
curl -D - -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/videos/trending
```

## Query parameters

The only query parameter that you should pass is the *mkt* parameter. Set to the market where the results come from, which is typically the market where the user is making the request from. For a list of markets that support trending videos, see [Market codes](../reference/market-codes.md#trending-video-api-markets).

You can ignore all other parameters listed in [Query parameters](../reference/query-parameters.md).

Here's a cURL example that includes the *mkt* query parameter.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/videos/trending?mkt=en-us
```

## Handling the response

When you send a request to Trending Videos API, it returns a [TrendingVideos](../reference/response-objects.md#trendingvideos) object in the response body. The **TrendingVideos** object contains a list of videos to highlight in the banner and a list of trending videos by category.

```json
{
  "_type": "TrendingVideos",
  "bannerTiles": [
    {
      "query": { ... },
      "image": { ... }
    },

    . . .
  ],
  "categories": [
    {
      "title": "Music videos",
      "subcategories": [
        {
          "tiles": [
            {
              "query": { ... },
              "image": { ... }
            },
          
            . . .  
          ],
          "title": "Top "
        },
        {
          "tiles": [
            {
              "query": { ... },
              "image": { ... }
            },
          
            . . .  
          ],
          "title": "Trending "
        },

        . . .
      ]
    },
    {
      "title": "Viral videos",
      "subcategories": [ ... ]
    },
    {
      "title": "TV shows",
      "subcategories": [ ... ]
    },
    {
      "title": "Movie trailers",
      "subcategories": [ ... ]
    }
  ]
}
```

### Display banner tiles

The `bannerTiles` field contains the list of [Tile](../reference/response-objects.md#tile) objects that you use to highlight videos in the banner of your UX.

```json
  "bannerTiles": [
    {
      "query": {
        "text": "<search query string>",
        "displayText": "<search query string with possible hit highlighting characters>",
        "webSearchUrl": "https:\/\/www.bing.com\/videos\/search?q=<search query string>&FORM=VSTREQ"
      },
      "image": {
        "description": "Image from: contoso.com",
        "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OET.84ceafb8856247968fc5f3a...",
        "headLine": "<description that describes the video's content>",
        "contentUrl": "https:\/\/www.contoso.com\/2019\/10\/23\/path\/"
      }
    },

    . . .
  ]
```

Use `thumbnailUrl` to display an image of the video. Use `description` to attribute the image. Use `headLine` to provide a description of the video. Options for making the thumbnail clickable:

- Use `contentUrl` to take the user to the provider's website where they can watch the video.
- Use `webSearchUrl` to take the user to Bing's Video search results where they can watch this and other videos.

### Displaying categories of trending videos

The `categories` field contains the lists of trending videos by category (see the [Category](../reference/response-objects.md#category) object). Use the category's `title` to group the videos in your user experience. For example, Music videos and Movie trailers.

```json
  "categories": [
    {
      "title": "Music videos",
      "subcategories": [ ... ]
    },
    {
      "title": "Viral videos",
      "subcategories": [ ... ]
    },
    {
      "title": "TV shows",
      "subcategories": [ ... ]
    },
    {
      "title": "Movie trailers",
      "subcategories": [ ... ]
    }
  ]
```

Each category is further subdivided into subcategories. For example, Music videos may be subdivided into Top music videos, Trending music videos, etc. Use the subcategory's `title` to group videos in your user experience.

```json
    {
      "title": "Music videos",
      "subcategories": [
        {
          "tiles": [ ... ],
          "title": "Top "
        },
        {
          "tiles": [ ... ],
          "title": "Trending "
        },
        {
          "tiles": [ ... ],
          "title": "More music videos "
        },
      ]
    },
```

The `tiles` field contains the list of [Tile](../reference/response-objects.md#tile) objects. Each tile is a trending video.

```json
      "subcategories": [
        {
          "tiles": [
            {
              "query": {
                "text": "<search query string>",
                "displayText": "<search query string with possible hit highlighting characters>",
                "webSearchUrl": "https:\/\/www.bing.com\/videos\/search?q=<search query string>&FORM=VSTREQ"
              },
              "image": {
                "description": "Image from: contoso.com",
                "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OET.84ceafb8856247968fc5f3a...",
                "contentUrl": "https:\/\/www.contoso.com\/2019\/10\/23\/path\/"
              }
            },
        
            . . .  
          ],
          "title": "Top "
        },
```

Use `thumbnailUrl` to display an image of the video. Use `description` to attribute the image. Options for making the thumbnail clickable:

- Use `contentUrl` to take the user to the provider's website where they can watch the video.
- Use `webSearchUrl` to take the user to Bing's Video search results where they can watch this and other videos.

## Next steps

- Learn about [use and display requirements](../../bing-web-search/use-display-requirements.md) for Bing Video Search.  
- Learn about [resizing and cropping thumbnails](../../bing-web-search/resize-and-crop-thumbnails.md).  
- Learn about [searching the web for videos](get-videos.md).
- Review [Video Search API v7 reference](../reference/endpoints.md) documentation.  
- Learn about the [quickstarts](../quickstarts/quickstarts.md) and [samples](../samples.md) that are available to help you get up and running fast.
