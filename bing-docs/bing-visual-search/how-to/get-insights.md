---
title: "Get insights about an image"
titleSuffix: Bing Search Services
description: This article describes how to get insights about an image using Bing Visual Search API.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 03/07/2023
---

# Get insights about an image

Bing Visual Search API returns insights about an image. For example, Bing can help you find similar images, learn where to buy the dress seen in the pic, explore a landmark, identify a dog’s breed, and more. All you need is an image file, the URL to an image, or an insights token.

> [!IMPORTANT]
> Bing Visual Search API is the preferred method for getting image insights.

If you have your subscription key, just send an HTTP POST request to the following endpoint:

`https://api.bing.microsoft.com/v7.0/images/visualsearch`

Here's a cURL example that shows you how to call the endpoint using your subscription key. Change the `url` field to any image URL that you want to get insights of. Make sure to escape quote marks (") and forward slashes (/).

```curl
curl -X POST -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -F knowledgeRequest="{\"imageInfo\":{\"url\":\"https://contoso.com/path/image.jpg\"}}"  https://api.bing.microsoft.com/v7.0/images/visualsearch?mkt=en-us
```

## Using boundary strings

The body of the POST contains form data. This means you must set the Content-Type header to `multipart/form-data; boundary=<boundarystringgoeshere>`. The boundary string may be any unique, opaque string that identifies the boundary of the form data. For example, boundary=boundary_1234-abcd

Use the boundary string to bound the start and end of the form data. You must include the double dashes as shown in the example below.

```curl

--boundary_1234-abcd

--boundary_1234-abcd--

```

Terminate each line with a carriage return and line feed character (\r\n).

The form data must include the Content-Disposition header. If you use a URL or insights token to specify the image, set the header’s *name* parameter to "knowledgeRequest".

```curl
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

--boundary_1234-abcd--
```

## Specifying the image's information

The body may include one or more parts of the following JSON object depending on how you specify the image.

```json
{
  "imageInfo" : {
    "url" : "",
    "imageInsightsToken" : "",
    "cropArea" : {
      "top" : 0.0,
      "left" : 0.0,
      "right" : 0.0,
      "bottom" : 0.0
    }
  },
  "knowledgeRequest" : {
    "filters" : {
      "site" : ""
    },
    "invokedSkillsRequestData" : {
      "enableEntityData" : "true"
    },
    "invokedSkills": [""],
    "offset":0,
    "count":0
  }
}
```

The **ImageInfo** object includes the following fields:

- `url` &mdash; The URL to the Internet-accessible image that you want insights of. The maximum supported image size is 1 MB. [Read more](#url-example).
- `imageInsightsToken` &mdash; The insights token that Image Search API returned for an image. [Read more](#image-token-example).
- `cropArea` &mdash; The bounding box that identifies the area of interest in the image. [Read more](#region-of-interest-example).

The `url` and `imageInsightsToken` fields are mutually exclusive. If the request uploads the image binary in the body of the request, don’t includes the `url` and `imageInsightsToken` fields.

The **KnowledgeRequest** object includes the following fields:

- `filters` &mdash; A **Filter** object that you use to filter the results.  
  - `site` &mdash; The *site* filter returns similar images and similar products results from the specified site only. [Read more](#filtering-similar-images-and-products).  
- `invokedSkillsRequestData` &mdash; A **SkillsRequestData** object that you use to request additional insights.  
  - `enableEntityData` &mdash; A Boolean value that if **true**, returns information about the well-known person, place, or thing seen in the image. [Read more](#including-entity-data).
- `invokedSkills` &mdash; A list of insights to invoke. The string array may include only SimilarImages or SimilarProducts but not both. Include this field if you want to page visually similar insights or images of visually similar products. [Read more](#paging).

## URL example

The following example shows what the body of the POST looks like if you use a URL to specify the image's location.

```curl
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "url" : "https://contoso.com/2018/05/fashion/red.jpg"
    }
}

--boundary_1234-abcd--
```

## Image token example

The following example shows what the body of the POST looks like if you specify an insights token.

```curl
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "imageInsightsToken" : "ccid_7qgerj5g*cp_96BBF7CD1AD0AE8E77A926647DF2266D*mid_644457F..."
    }
}

--boundary_1234-abcd-- 
```

You get the insights token from a previous call to [Image Search API](../../bing-image-search/overview.md). The following fragment of an **Image** object shows what the insight's token looks like (see the `imageInsightsToken` field).

```json
    {
      "name": "Elegant Black Cocktail Dress...",
      "thumbnailUrl": "https://tse1.mm.bing.net/th?id=OIP.7qgerj5gq7fbynq3ICwwHaHa&pid=Api",
      "contentUrl": "https://contoso.com/kf/HTB1DwMiSpXXXXaDXpXXq6xXFXXXg/Elegant-Black...",
      "hostPageUrl": "https://contoso.com/item/Elegant-Black-Cocktail...",
      "thumbnail": {
        "width": 474,
        "height": 474
      },
      "imageInsightsToken": "ccid_7qgerj5g*cp_96BBF7CD1AD0AE8E77A926647DF2266D*mid_644457F...",
      "insightsMetadata": {
        "shoppingSourcesCount": 1,
        "recipeSourcesCount": 0,
        "aggregateOffer": {
          "name": "Ever pretty fit and flare...",
          "priceCurrency": "USD",
          "lowPrice": 20.99,
          "offerCount": 1
        },
        "pagesIncludingCount": 11,
        "availableSizesCount": 4
      }
    },
```

## Region of interest example

By default, the API uses the entire image to look for insights about the image. But what if the picture contains multiple people and you want to know about only one of the people or you just want to know where to buy the watch seen in the image? In this case, you can use the `cropArea` field in the **ImageInfo** object to specify a region of interest.

The crop area specifies the top-left corner and bottom-right corner of a region of interest. Specify the values in the range 0.0 through 1.0. The values are a percentage of the overall width and height of the image.

```curl
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
  "imageInfo" : {
    "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CFEDF3D40...",
    "cropArea" : {
      "top" : 0.3,
      "left" : 0.4,
      "right" : 0.6,
      "bottom" : 0.5
    }
  },
  "knowledgeRequest" : {
    "filters" : {
      "site" : "www.contoso.com"
    }
  }
}

--boundary_1234-abcd-- 
```

## Including entity data

If the image might include a well-known person, place, or thing and you want to include entity insights in the response, set the `enableEntityData` field to `true`.

```curl
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
  "imageInfo" : {
      "url" : "https://contoso.com/2018/05/fashion/red.jpg"
  },
  "knowledgeRequest" : {
    "invokedSkillsRequestData" : {
        "enableEntityData" : "true"
    }
  }
}

--boundary_1234-abcd--
```

## Filtering similar images and products

If the user is shopping for a black cocktail dress and wants to see if the store they shop at carries it, use the `site` filter. The `site` filter restricts the similar images and similar products results to a specific domain.

```curl
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
  "imageInfo" : {
    "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CFEDF3D40..."
  },
  "knowledgeRequest" : {
    "filters" : {
      "site" : "www.contoso.com"
    }
  }
}

--boundary_1234-abcd-- 
```

## <a name="paging"></a> Paging VisualSearch and ProductVisualSearch action types

The VisualSearch and ProductVisualSearch action types may include the following paging-related fields.

```json
          "currentOffset": 0,
          "nextOffset": 237,
          "totalEstimatedMatches": 896
```

If the object includes these fields, and you want to provide your user the ability to page through more visually similar images or products, include the **KnowledgeRequest** object in your POST request with the following fields.

```json
  "knowledgeRequest" : {
    "invokedSkills": ["SimilarImages"],
    "offset":237,
    "count":35
  }
```

To paginate visually similar images, set the `invokedSkills` array to SimilarImages or set it to SimilarProducts to paginate visually similar products.

Set the `offset` field to the value from the `nextOffset` field in the VisualSearch or ProductVisualSearch insight depending on the value you specified in `invokedSkills`.

Set the `count` field to the number of images to return.

## Local image example

If you want to get insights about a local image you have, you can upload the image as binary data. The maximum image size you can upload is 1 MB and the maximum width and height should be 1,500 pixels or less.

The form data must include the Content-Disposition and Content-Type headers. For the Content-Disposition header, set the header’s *name* parameter to "image" and the *filename* parameter to any appropriate string. The Content-Type header may be set to any commonly used image mime type.

```curl
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

--boundary_1234-abcd--
```

The contents of the form is the image's binary data.

```curl
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

The following example shows how to specify the region of interest of an uploaded image.

```curl
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "cropArea" : {
            "top" : 0.2,
            "left" : 0.3,
            "bottom" : 0.7,
            "right" : 0.6
        }
    }
}

--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="image"
Content-Type: image/jpeg


ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## Next steps

- Learn about the [response](search-response.md) that Bing returns.
- Learn about the [quickstarts](../quickstarts/quickstarts.md) and [samples](../samples.md) that are available to help you get up and running fast.
