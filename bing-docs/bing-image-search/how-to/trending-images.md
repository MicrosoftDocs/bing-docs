---
title: Get trending images with the Bing Trending Images API
titleSuffix: Bing Search Services
description: Search for today's trending images from the web with the Bing Trending Images API.
services: bing-search-services
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: conceptual
author: alekhyasasi
ms.date: 04/05/2022
ms.author: v-apunnamara
---

# Get images that are trending

If you’re building a user experience that shows images that are trending on social media in different categories, use this API. This API offers an experience similar to bing.com/images/trending.

Calling the API is easy. If you have your subscription key, just send an HTTP GET request to the following endpoint:

```
https://api.bing.microsoft.com/v7.0/images/trending
```

Here's a cURL example that shows you how to call the endpoint using your subscription key. 

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/images/trending
```

For a list of markets that support Trending Images, see [Market codes](../reference/market-codes.md#trending-image-api-markets).  


## Request and response headers

Although that's all the more you need to do to get trending images, Bing does suggest you include a couple of other headers to provide a better experience for your user. Those headers include:

- User-Agent &mdash; Lets Bing know whether needs a mobile or desktop experience.
- X-MSEdge-ClientID &mdash; Provides continuity of experience.
- X-MSEdge-ClientIP &mdash; Provides the user's location for location aware queries.
- X-Search-Location &mdash; Provides the user's location for location aware queries.

The more information you can provide Bing, the better the experience will be for your users. To learn more about these headers, see [Request headers](../reference/headers.md#request-headers).

Here's a cURL example that includes these headers.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -H "X-MSEdge-ClientID: 00B4230B74496E7A13CC2C1475056FF4" -H "X-MSEdge-ClientIP: 11.22.33.44" -H "X-Search-Location: lat:55;long:-111;re:22" -A "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.65 Safari/537.36" https://api.bing.microsoft.com/v7.0/images/trending
```

Bing returns a couple of headers you should capture. 

- BingAPIs-TraceId &mdash; ID that identifies the request in the log file.
- X-MSEdge-ClientID &mdash; The ID that you need to pass in subsequent request to provide continuity of experience.
- BingAPIs-Market &mdash; The market used by Bing for the request.

To learn more about these headers, see [Response headers](../reference/headers.md#response-headers).

Here's a cURL call that returns the response headers. If you want to remove the response data so you can see only the headers, include the `-o nul` parameter.

```curl
curl -D - -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/images/trending
```


## Query parameters

The only query parameter that you should pass is the *mkt* parameter. Set to the market where the results come from, which is typically the market where the user is making the request from.

You can ignore all other parameters listed in [Query parameters](../reference/query-parameters.md).

Here's a cURL example that includes the *mkt* query parameter.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/images/trending?mkt=en-us
```


## Handling the response

When you send a request to Trending Images API, it returns a [TrendingImages](../reference/response-objects.md#trendingimages) object in the response body. The **TrendingImages** object contains the lists of trending images by category. Use the category's `title` to group the images in your user experience. The categories may change daily.

```json
{
  "_type": "TrendingImages",
  "categories": [
    {
      "title": "Popular people searches",
      "tiles": [...]
    },
    {
      "title": "Popular animal searches",
      "tiles": [...]
    },
    {
      "title": "Popular nature and landmark searches",
      "tiles": [...]
    },
    {
      "title": "Popular wallpaper searches",
      "tiles": [...]
    },
    {
      "title": "Popular gif searches",
      "tiles": [...]
    }
  ]
}
```

Each [Category](../reference/response-objects.md#category) object includes the following fields:

- `title` &mdash; The name of the category. Use the name to group the images in your UX.
- `tiles` &mdash; A list of [Tile](../reference/response-objects.md#tile) objects. 

Each tile contains a trending image and options for getting related images. The following example shows what a tile looks like in the JSON response.

```json
        {
          "query": {
            "text": "Red-knobbed hornbill",
            "displayText": "Red-knobbed hornbill",
            "webSearchUrl": "https://www.bing.com/images/search?q=Red-knobbed+hornbill..."
          },
          "image": {
            "thumbnailUrl": "https://tse3.mm.bing.net/th?id=OET.c5cf...",
            "contentUrl": "http://contoso.com/2011...",
            "width": 1001,
            "height": 1200,
            "thumbnail": {
              "width": 474,
              "height": 568
            },
            "imageId": "467A365F7E182F8875EB3379B3B1E8DB4FB2C464"
          }
        },
```

The [Image](../reference/response-objects.md#image) contains the trending image to display on the tile and the [Query](../reference/response-objects.md#query) object contains the option for getting related images.

To get the related images, use the query string in the `text` field to call Image Search API and display the related images yourself. Or, use the URL in `webSearchUrl` to take the user to Bing's search results page to see the related images.

If you call Image Search API to get the related images, set the [id](../reference/query-parameters.md#id) query parameter to the ID in the `imageId` field. Specifying the ID ensures that the response contains the image (it is the first image in the response) and its related images. Also, set the [q](../reference/query-parameters.md#query) query parameter to the text in the **Query** object's `text` field.

The following cURL example shows how to call Image Search API to get the related images.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/v7.0/images/search?q=Red-knobbed+hornbill&id=467A365F7E182F8875EB3379B3B1E8DB4FB2C464&mkt=en-us
```


## Next steps

- Learn about [use and display requirements](../../bing-web-search/use-display-requirements.md) for Bing Image Search.  
- Learn about [resizing and cropping thumbnails](../../bing-web-search/resize-and-crop-thumbnails.md).  
- Learn about [searching the web for images](get-images.md).
- Review [Image Search API v7 reference](../reference/endpoints.md) documentation.  
- Learn about the [quickstarts](../quickstarts/quickstarts.md) and [samples](../samples.md) that are available to help you get up and running fast.

