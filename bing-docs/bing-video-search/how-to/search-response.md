---
title: "Send search requests to the Bing Video Search API"
titleSuffix: Bing Search Services
description: This article describes the parameters and attributes of requests sent to the Bing Video Search API, as well as the JSON response object it returns.
services: bing-search-services
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-video-search
ms.topic: conceptual
author: alekhyasasi
ms.date: 07/11/2023
---

# Handle the video search response

When you send a request to Video Search API, it returns a [VideosAnswer](../reference/response-objects.md#videosanswer) object in the response body. The object may include one or more of the following fields:

```json
{
  "_type": "Videos",
  "readLink": "https://api.bing.microsoft.com/api/v7/videos/search?q=sailing+dinghies",
  "webSearchUrl": "https://www.bing.com/videos/search?q=sailing+dinghies",
  "queryContext": { ... },
  "totalEstimatedMatches": 835,
  "nextOffset": 36,
  "currentOffset": 0,
  "value": [ { ... } ],
  "pivotSuggestions": [ { ... } ],
  "relatedSearches": [ { ... } ]
}
```

But if an error occurs, the response body contains an [ErrorResponse](../reference/response-objects.md#errorresponse) object. Bing returns an error response for all 400 level HTTP status codes. [Read more](../reference/error-codes.md).

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

For information about the `nextOffset` and `totalEstimatedMatches` fields, see [Paging image and video results](../../bing-web-search/page-results.md#paging-image-and-video-results).

## Displaying videos

The `value` field contains the list of [Video](../reference/response-objects.md#video) objects that Bing thought were relevant. Here's what the **Video** object looks like in the response.

```json
    {
      "webSearchUrl": "https://www.bing.com/videos/search?q=sailing%20dinghies...",
      "name": "Getting Started - Dinghy Sailing - with...",
      "description": "What to do when your small boat capsizes. Capsizing is a common occurrence...",
      "thumbnailUrl": "https://tse2.mm.bing.net/th?id=OVP._9LeWLNdwQBZ8KKAIx...",
      "datePublished": "2012-07-25T13:23:33.0000000",
      "publisher": [
        {
          "name": "Fabrikam"
        }
      ],
      "creator": {
        "name": "Contoso Yachting Association"
      },
      "isAccessibleForFree": true,
      "isFamilyFriendly": true,
      "contentUrl": "https://www.fabrikam.com/watch?v=EfJIu_mo",
      "hostPageUrl": "https://www.fabrikam.com/watch?v=EfJIu_mo",
      "encodingFormat": "h264",
      "hostPageDisplayUrl": "https://www.fabrikam.com/watch?v=EfJIu_mo",
      "width": 1280,
      "height": 720,
      "duration": "PT3M12S",
      "motionThumbnailUrl": "https://tse1.mm.bing.net/th?id=OM.aBjM7NNnt5GKYA_1595...",
      "embedHtml": "<iframe width=\"1280\" height=\"720\" src=\"https://www.fabrikam.com/embed/EfJIu_mo?autoplay=1\" frameborder=\"0\" allowfullscreen></iframe>",
      "allowHttpsEmbed": true,
      "viewCount": 148295,
      "thumbnail": {
        "width": 300,
        "height": 225
      },
      "videoId": "608A91B767D3ECCC1868608A91B767DC1868",
      "allowMobileEmbed": true,
      "isSuperfresh": false
    },
```

If you plan to display a collage of the videos on a page, use the following fields:

- `thumbnailUrl` &mdash; A URL to the thumbnail of the video that `contentUrl` points to.
- `thumbnail` &mdash; The size of the thumbnail that `thumbnailUrl` points to.

For information about resizing the thumbnail, see [Resizing and cropping thumbnails](../../bing-web-search/resize-and-crop-thumbnails.md).

You should display the following information with each thumbnail:

- `duration` &mdash; The video's length. Convert the [duration notion](https://en.wikipedia.org/wiki/ISO_8601#Durations) to human-readable form. For example, convert PT3M12S to 3:12.
- `name` &mdash; The video's title.
- `datePublished` &mdash; The date that Bing discovered the video. Use the date portion of the date time stamp.
- `publisher` &mdash; The publisher that's hosting the video.
- `creator` &mdash; The person or organization that created the video.
- `viewCount` &mdash; The number of times the video has been watched on the publisher's website. Gives an indication of how popular the video is.
- `isAccessibleForFree` &mdash; A Boolean value that indicates whether the video is free to view. If **false**, add a currency sign when you display the video's information.

If you truncate any of the strings to fit your UX, append an ellipsis.

As the user hovers over the thumbnail, use `motionThumbnailUrl`, if it exists, to play a thumbnail version of the video. Add a label over the motion thumbnail as it plays that says, **Preview**. If the **Video** object doesn't include `motionThumbnailUrl`, add a label over the thumbnail as the user hovers over it that says, **No preview**.

Make the surface where you display the thumbnail and video information clickable by using one of the following URLs:

- Use `hostPageUrl` to view the video on the provider’s website.
- Use `webSearchUrl` to view the video in Bing’s video viewer.
- Use `embedHtml`, if it exists, to view the video in your own experience. If you provide your own view experience, consider `allowHttpsEmbed` and `allowMobileEmbed`.

## Using pivot queries

If Bing can segment the user’s query, the response includes the `pivotSuggestions` field, which is a list of [Pivot](../reference/response-objects.md#pivot) objects. Each **Pivot** object contains one of the segments from the user’s query (see the `pivot` field). For example, if the user's query is *sailing dinghies*, the pivots are *sailing* and *dinghies*. Basically, Bing replaces each pivot word in the user's query with another term. The following list contains some of the query suggestions for the *dinghies* pivot.

- Sailing **Small Boats**
- Sailing **Rigging**
- Sailing **Yachts**

As you can see from this example, Bing may not provide suggestions for all pivots.

```json
  "pivotSuggestions": [
    {
      "pivot": "sailing",
      "suggestions": []
    },
    {
      "pivot": "dinghies",
      "suggestions": [
        {
          "text": "Sailing Small Boats",
          "displayText": "Small Boats",
          "webSearchUrl": "https://www.bing.com/videos/search?q=Sailing+Small+Boats...",
          "searchLink": "https://api.bing.microsoft.com/v7/videos/search?q=Sailing+Small+Boats...",
          "thumbnail": {
            "thumbnailUrl": "https://tse4.mm.bing.net/th?q=Sailing+Small+Boats&pid=Api..."
          }
        },
        {
          "text": "Sailing Rigging",
          "displayText": "Rigging",
          "webSearchUrl": "https://www.bing.com/videos/search?q=Sailing+Rigging...",
          "searchLink": "https://api.bing.microsoft.com/v7/videos/search?q=Sailing+Rigging...",
          "thumbnail": {
            "thumbnailUrl": "https://tse1.mm.bing.net/th?q=Sailing+Rigging&pid=Api..."
          }
        },

        . . .
      ]
    }
  ]
```

The `pivotSuggestions` field contains the list of segments (pivots) that the original query was broken into. For each pivot, the response contains a list of [Query](../reference/response-objects.md#query) objects that contain suggested queries. The `text` field contains the suggested query. The `displayText` field contains the term that replaces the pivot in the original query.

Make `text` clickable by using the URL in `webSearchUrl` or `searchLink`. Use `webSearchUrl` to send the user to Bing's Video search results. If you provide your own results page experience, use `searchLink` to get new video search results using the pivot query string.

## Related searches answer

The `relatedSearches` field contains a list of the most popular related queries made by other users. Each [query](../reference/response-objects.md#query) in the list includes a query string (`text`), a query string with hit highlighting characters (`displayText`), and a URL (`webSearchUrl`) to Bing's Video search results page for that query.

```json
  "relatedSearches": [
    {
      "text": "Sailing Dinghy for Beginners",
      "displayText": "Sailing Dinghy for Beginners",
      "webSearchUrl": "https://www.bing.com/videos/search?q=Sailing+Dinghy+for+Beginners&FORM=VRPATC",
      "searchLink": "https://api.bing.microsoft.com/v7/videos/search?q=Sailing+Dinghy+for+Beginners",
      "thumbnail": {
        "thumbnailUrl": "https://tse1.mm.bing.net/th?q=Sailing+Dinghy+for+Beginners&pid=Api..."
      }
    },
    {
      "text": "Sailing Boats",
      "displayText": "Sailing Boats",
      "webSearchUrl": "https://www.bing.com/videos/search?q=Sailing+Boats&FORM=VRPATC",
      "searchLink": "https://api.bing.microsoft.com/v7/videos/search?q=Sailing+Boats",
      "thumbnail": {
        "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Sailing+Boats&pid=Api..."
      }
    },

    . . .
  ]
```

Use the `displayText` query string and the `webSearchUrl` URL to create a hyperlink that takes the user to Bing's Video search results page for the related query. If you provide your own results page experience, use `searchLink` to get new video search results using the related query string.

For information about how to handle the highlighting markers in `displayText`, see [Hit Highlighting](../../bing-web-search/hit-highlighting.md).

## Next steps

- Learn how to [get trending videos](trending-videos.md).
- Learn how to [get insights about a video](video-insights.md) such as related videos.
