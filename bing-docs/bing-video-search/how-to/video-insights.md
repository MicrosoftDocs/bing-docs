---
title: Get video insights using the Bing Video Search API
titleSuffix: Bing Search Services
description: Learn how to use the Bing Video Search API to get more information about videos, such as related videos.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Get insights about a video

Sometimes users want to get information about a video, such as getting related videos. Each [Video](../reference/response-objects.md#video) object includes a video ID that you use to get insights. The following **Video** object fragment shows the `videoId` field that contains the ID.

```json
  "value" : [
    {
      . . .
      "name" : "How to sail - What to Wear for Dinghy Sailing",
      "description" : "An informative video on what to wear...",
      "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjHBZ--g",
      "videoId" : "6DB795E11A6E3CB...",
      . . .
    }
  ],
```

After getting the ID, send an HTTP GET request to Video Insights API. Set the [id](../reference/query-parameters.md#id) query parameter to the ID in `videoId`. Set the [modules](../reference/query-parameters.md#modulesrequested) query parameter to the insights you want to get. To get all insights, set `modules` to All. The response includes all the insights that you requested, if available.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/videos/details?id=6DB795E11A6E3CB...&modules=All&mkt=en-us  
```

The top-level object in the response is a [VideoDetails](../reference/response-objects.md#videodetails) object. The following fragment shows what it might look like.

```json
{
  "_type": "Api.VideoDetails.VideoDetails",
  "relatedVideos": { ... },
  "videoResult": { ... }
}
```

The `relatedVideos` field contains a list of videos that are related to the specified videos and the `videoResult` field contains the video that the insights are based on.


## Getting related videos insights  

To get videos that are related to the specified video, set the [modules](../reference/query-parameters.md#modulesrequested) query parameter to `RelatedVideos`.
  
```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/videos/details?id=6DB795E11A6E3CB...&modules=relatedvideos&mkt=en-us  
```

```json
  "relatedVideos": {
    "value": [
      {
        "webSearchUrl": "https:\/\/www.bing.com\/videos\/search?q=&view=detail&mid=B84EB61E20A24C7F09F2B84EB61E20A24C7F09F2",
        "name": "Solo sailing Los Angeles to Hawaii on 23ft boat",
        "description": "Instagram @SamHolmesSailing This video documents the 27 days alone at sea aboard my small 23ft sailing yacht. I sailed 2100 nautical miles from Los Angeles California to Hilo Hawai’i. This was my first big ocean crossing. The boat was a Ranger 23. If you’re interested in contributing to the next adventure, links for patreon, paypal and ...",
        "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?id=OVP.i57jQTjnUvZDfgjYJzjDegEsDh&pid=Api",
        "datePublished": "2019-07-17T22:15:26.0000000",
        "publisher": [
          {
            "name": "YouTube"
          }
        ],
        "creator": {
          "name": "Sam Holmes Sailing"
        },
        "isAccessibleForFree": true,
        "isFamilyFriendly": true,
        "contentUrl": "https:\/\/www.youtube.com\/watch?v=yUi0gsxVHZM",
        "hostPageUrl": "https:\/\/www.youtube.com\/watch?v=yUi0gsxVHZM",
        "encodingFormat": "h264",
        "hostPageDisplayUrl": "https:\/\/www.youtube.com\/watch?v=yUi0gsxVHZM",
        "width": 1280,
        "height": 720,
        "duration": "PT39M45S",
        "motionThumbnailUrl": "https:\/\/tse2.mm.bing.net\/th?id=OM.8gl_TKIgHrZOuA_1592594362&pid=Api",
        "embedHtml": "<iframe width=\"1280\" height=\"720\" src=\"https:\/\/www.youtube.com\/embed\/yUi0gsxVHZM?autoplay=1\" frameborder=\"0\" allowfullscreen><\/iframe>",
        "allowHttpsEmbed": true,
        "viewCount": 2914001,
        "thumbnail": {
          "width": 300,
          "height": 225
        },
        "videoId": "B84EB61E20A24C7F09F2B84EB61E20A24C7F09F2",
        "allowMobileEmbed": true,
        "isSuperfresh": false
      },

      . . .
    ]
  }
```

For information about displaying the videos that Bing returns, see [Displaying videos](search-response.md#displaying-videos).


## Next steps

- Learn how to [get trending videos](trending-videos.md).
- Learn how to [search the web for videos](get-videos.md).
- Learn about the [quickstarts](../quickstarts/quickstarts.md) and [samples](../samples.md) that are available to help you get up and running fast.

