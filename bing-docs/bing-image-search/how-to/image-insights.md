---
title: Get image insights
titleSuffix: Bing Search Services
description: Learn how to use the Bing Image Insights API to get more information about an image.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Get image insights with Bing Image Insights API

> [!IMPORTANT]
> Because [Bing Visual Search API](../../bing-visual-search/overview.md) provides more comprehensive image insights, use it instead of Bing Image Insights API.

Each image includes an insights token that you can use to get information about the image. For example, you can get a collection of related images, web pages that include the image, or a list of merchants where you can buy the product shown in the image.  

To get insights about an image, get the image's [imageInsightsToken](../reference/response-objects.md#image-imageinsightstoken) token in the response.

```json
"value" : [
    {
      . . .
      "name":"sailing dinghy.jpg",
      "imageInsightsToken" : "mid_D6426898706EC7..."
      "insightsSourcesSummary" : {
        "shoppingSourcesCount" : 9,
        "recipeSourcesCount" : 0
      },
      . . .
    }
  ],
```

Next, call Image Insights API and set the [insightsToken](../reference/query-parameters.md#insightstoken) query parameter to the token in the `imageInsightsToken` field.  

To specify the insights that you want to get, set the [modules](../reference/query-parameters.md#modulesrequested) query parameter. To get all insights, set *modules* to `All`. To get only the caption and collection insights, set *modules* to `Caption%2CCollection`. 

The following cURL example requests all available insights using the token from the above example.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/images/details?q=mt+rainier&insightsToken=mid_D6426898706EC7...&modules=All&mkt=en-us  
```

As you can see from the response, not all images return all insights. The response includes all insights that you requested, only if available.

```json
{
  "_type": "ImageInsights",
  "imageInsightsToken": "mid_D6426898706EC7...",
  "bestRepresentativeQuery": { ... },
  "relatedSearches": { "value": [ ... ] },
  "visuallySimilarImages": { "value": [ ... ] },
  "image": { ... }
```


## Getting insights of a known image

If you have the URL to an image that you want to get insights of, use the [imgUrl](../reference/query-parameters.md#imgurl) query parameter instead of the [insightsToken](../reference/query-parameters.md#insightstoken) parameter to specify the image. Or, if you have the image file, you may send the binary of the image in the body of a POST request. If you use a POST request, the Content-Type header must be set to `multipart/data-form`. With either option, the size of the image may not exceed 1 MB.  

The following example shows how to request insights of an image using the *imgUrl* parameter.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/images/details?q=sun+flower&&imgUrl=https%3A%2F%2Fwww.mydomain.com%2Fimages%2Fsunflower.png&modules=All&mkt=en-us  
```


## Getting all image insights  

To request all insights of an image, set the [modules](../reference/query-parameters.md#modulesrequested) query parameter to `All`. To get related searches, the request must include the user's query string. This example shows using the [insightsToken](../reference/query-parameters.md#insightstoken) to specify the image.  

```
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&insightsToken=mid_68364D764J...&modules=All&mkt=en-us
```

The top-level object is an [ImageInsightsResponse](../reference/response-objects.md#imageinsightsresponse) object instead of an [ImageAnswer](../reference/response-objects.md#imageanswer) object.  

```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "bcid_3297E6A54E4787A5F51C49D9BA342B9A*ccid_Fe2Hx...",
    "bestRepresentativeQuery" : {
        "text" : "Sailing Dinghy",
        "displayText" : "Sailing Dinghy",
        "webSearchUrl" : "https://www.bing.com/images/search?q=Sailing+Dinghy...",
    },
    "pagesIncluding" : {
        "value" : [
            {
                "webSearchUrl" : "https://www.bing.com/images/search?view=...",
                "name" : "Powerboating Dublin, Dinghy Sailing Courses...",
                "thumbnailUrl" : "https://tse1.mm.bing.net/th?id=OIP....",
                "datePublished" : "2017-01-20T00:41:00.0000000Z",
                "contentUrl" : "http://www.contoso.ie/content...",
                "hostPageUrl" : "http://www.contoso.ie/powerboating...",
                "contentSize" : "59063 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "www.contoso.ie/powerboating...",
                "width" : 800,
                "height" : 600,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 225
                },
                "imageInsightsToken" : "ccid_pHjQIA0x*mid_17F61B1316A39C92214...",
                "imageId" : "17F61B1316A39C922143FFDE9DFB5B0FB41171",
                "accentColor" : "0997C2"
            },
            . . .
        ]
    },
    "relatedSearches" : {
        "value" : [
            {
                "text" : "Sailing Fun",
                "displayText" : "Sailing Fun",
                "webSearchUrl" : "https://www.bing.com/images/search?q=Sailing...",
                "thumbnail" : {
                    "url" : "https://tse1.mm.bing.net/th?q=Sailing+Fun..."
                }
            },
            . . .
        ]
    },
    "visuallySimilarImages" : {
        "value" : [
            {
                "webSearchUrl" : "https://www.bing.com/images/search?view=...",
                "name" : "Weekend On the Water",
                "thumbnailUrl" : "https://tse2.mm.bing.net/th?id=OIP...",
                "datePublished" : "2010-09-05T12:00:00.0000000Z",
                "contentUrl" : "http://1.bp.contoso.com/_dc_6...",
                "hostPageUrl" : "http://contoso.com/2010...",
                "contentSize" : "203806 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "contoso.com/2010...",
                "width" : 1600,
                "height" : 1249,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 234
                },
                "imageInsightsToken" : "ccid_Jg1Kwuc4*mid_5B7DA43976D3A422...",
                "imageId" : "5B7DA43976D3A422BA679A3AB019BB52C08DBC",
                "accentColor" : "0B2543"
            },
            . . .
        ]
    },
    "imageTags" : {
        "value" : [
            {
                "name" : "sail boat"
            },
            . . .
        ]
    }
}
```


## Recognizing entities in an image  

The entity recognition feature identifies well-known people in an image. To identify entities in an image, set the [modules](../reference/query-parameters.md#modulesrequested) query parameter to `RecognizedEntities`.  

> [!NOTE]
> You may not specify this module with any other module. If you specify this module with other modules, the response does not include recognized entities.  

The following shows how to specify the image by using the [imgUrl](../reference/query-parameters.md#imgurl) parameter. Remember to URL encode the query parameters.  

```
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/images/details?q=bill+gates&insightsToken=mid_68364D764J...&modules=RecognizedEntities&mkt=en-us
```  

The following shows the response to the previous request. Because the image contains two people, the response identifies a region for each person. In this case, the people were recognized in the CelebrityAnnotations and CelebRecognitionAnnotations groups. Bing lists the people in each group based on the likelihood that they match the person in the original image. The list is in descending order of confidence. The CelebRecognitionAnnotations group provides the highest level of confidence that the match is correct.  

```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "ccid_ldi5nX38*mid_29470780DE0E6F969...",
    "recognizedEntityGroups" : {
        "value" : [
            {
                "recognizedEntityRegions" : [...],
                "name" : "CelebRecognitionAnnotations"
            },
            {
                "recognizedEntityRegions" : [...],
                "name" : "CelebrityAnnotations"
            }
        ]
    }
}
```

The `region` field identifies the area of the image where Bing recognized the entity. For people, the region represents the person's face.  

The values of the rectangle are relative to the width and height of the original image and are in the range 0.0 through 1.0. For example, if the image is 300x200, and the region's top, left corner is at point (10, 20) and the bottom, right corner is at point (290, 150), then the normalized rectangle is:  

- Left: 10 / 300 = 0.03333...  
- Top:  20 / 200 = 0.1  
- Right: 290 / 300 = 0.9667...  
- Bottom: 150 / 200 = 0.75  

You can use the region that Bing returns in subsequent insights calls. For example, to get visually similar images of the recognized entity. For more information, see [Cropping Images to use with Visually Similar and Entity Recognition modules](#cropping-images-to-use-with-visually-similar-and-entity-recognition-modules). The following shows the mapping between the region fields and the query parameters that you'd use to crop images.  

- Left maps to [cal](../reference/query-parameters.md#cal)  
- Top maps to [cat](../reference/query-parameters.md#cat)  
- Right maps to [car](../reference/query-parameters.md#car)  
- Bottom maps to [cab](../reference/query-parameters.md#cab)  


## Finding visually similar images  

To find images that are visually similar to the original image, set the [modules](../reference/query-parameters.md#modulesrequested) query parameter to SimilarImages.  

The following request shows how to get visually similar images. The request uses the [insightsToken](../reference/query-parameters.md#insightstoken) query parameter to identify the original image. To improve relevance, include the user's query string.  

```
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/images/details?insightsToken=mid_68364D764J...&modules=SimilarImages&mkt=en-us&hawaiian+sunset
```

The following shows what the response to the previous request looks like.  

```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "mid_68364D764J...",
    "visuallySimilarImages" : {
        "value" : [
            {
                "name" : "typical Hawaiian Sunset! :) | Scenes of Hawaii",
                "webSearchUrl" : "https://www.bing.com/images/search?view=detailv2...",
                "thumbnailUrl" : "https://tse1.mm.bing.net/th?id=OIP.Mda2a86...",
                 . . .
            }
        ]
    }
```


## Cropping images to use with visually similar and entity recognition modules  

To specify the region of the image that Bing uses to determine whether images are visually similar or to perform entity recognition, use the [cal](../reference/query-parameters.md#cal), [cat](../reference/query-parameters.md#cat), [cab](../reference/query-parameters.md#cab), and [car](../reference/query-parameters.md#car) query parameters. By default, Bing uses the entire image.  

The parameters specify the top, left corner and bottom, right corner of the region that Bing uses for comparison. Specify the values as fractions of the original image's width and height. The fractional values start with (0.0, 0.0) at the top, left corner and end with (1.0, 1.0) at the bottom right corner. For example, to specify that the top, left corner starts a quarter of the way down from the top and a quarter of the way in from the left side, set `cal` to 0.25 and `cat` 0.25.  

The following sequence of calls shows the effect of specifying the cropping region. The first call does not include cropping and Bing recognizes two people standing side by side in the middle of the image.  

```  
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/images/details?modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us
```

The response shows two recognized entities.  

```json
{  
    "_type" : "ImageInsights",  
    "recognizedEntityGroups" : {
        "value": [  
            . . .  
            {  
                "recognizedEntityRegions" : [{  
                    "region" : {  
                        "left" : 0.5066667,  
                        "top" : 0.1955556,  
                        "right" : 0.75,  
                        "bottom" : 0.52  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Charlene Whitney",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                },  
                {  
                    "region" : {  
                        "left" : 0.25,  
                        "top" : 0.2488889,  
                        "right" : 0.4466667,  
                        "bottom" : 0.5111111  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Marcus Appel",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                }],  
            "name" : "CelebRecognitionAnnotations"
        }]
    }  
}  
```  

The second call crops the image vertically down the middle and Bing recognized a single person on the right side of the image.  

```
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/images/details?cal=0.5&cat=0.0&car=1.0&cab=1.0&modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us
```

The response shows one recognized entity.  

```json  
{  
    "_type" : "ImageInsights",  
    "recognizedEntityGroups" : {
        "value" : [  
            . . .  
            {  
                "recognizedEntityRegions" : [{  
                    "region" : {  
                        "left" : 0.5066667,  
                        "top" : 0.1955556,  
                        "right" : 0.75,  
                        "bottom" : 0.52  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Charlene Whitney",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                }],  
                "name" : "CelebRecognitionAnnotations"  
            }
        ]
    }
}  
```  

## Finding visually similar products  

To find images that contain products that are visually similar to the products found in the original image, set the [modules](../reference/query-parameters.md#modulesrequested) query parameter to SimilarProducts.  

The following request shows how to get images of visually similar products. The request uses the [insightsToken](../reference/query-parameters.md#insightstoken) query parameter to identify the original image that was returned in a previous request. To improve relevance, you should include the user's query string.  

```
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/images/details?q=black+cocktail+dresses&modules=SimilarProducts&insightsToken=ccid_WOeyfoSp*mid_4B0A357...&mkt=en-us
```

The following example shows what to the visually similar response looks like. The response contains an image of a similar product and indicates how many merchants offer the product online, whether there are product ratings, and the lowest price found (see the `aggregateOffer` field).  

```json
{
  "_type" : "ImageInsights",
  "imageInsightsToken" : "ccid_WOeyfoSp*mid_4B0A357...",
  "visuallySimilarProducts" : {
    "value" : [
      {
        "webSearchUrl": "https://www.bing.com/images/search?view=detailv2&FORM=OIIRPO&q=black...",
        "name": "Black Simple V-neck Sleeveless Zipper Knee Length...",
        "thumbnailUrl": "https://tse3.mm.bing.net/th?id=OIP.VFnOfyyprhy1REL...",
        "datePublished": "2019-10-11T23:38:00.0000000Z",
        "isFamilyFriendly": true,
        "contentUrl": "http://www.contoso.com/media/catalog/product/21010/black/r26_1.jpg",
        "hostPageUrl": "http://www.contoso.com/p/simple-v-neck-sleeveless...",
        "contentSize": "117683 B",
        "encodingFormat": "jpeg",
        "hostPageDisplayUrl": "www.contoso.com/p/simple-v-neck-sleeveless...",
        "width": 975,
        "height": 1500,
        "thumbnail": {
          "width": 474,
          "height": 729
        },
        "imageInsightsToken": "ccid_VFnOfyyp*cp_088F3195...",
        "insightsMetadata": {
          "shoppingSourcesCount": 1,
          "recipeSourcesCount": 0,
          "aggregateOffer": {
            "name": "Chocolate brown Informal Zipper Chiffon Knee Length Sequin Bridesmaid Dresses",
            "priceCurrency": "USD",
            "lowPrice": 75.99,
            "offerCount": 1
          },
          "pagesIncludingCount": 69,
          "availableSizesCount": 15
        },
        "imageId": "E240818411399703235868186EFE139522E31B7B",
        "accentColor": "735158"
      },

      . . .
    ]
  }
}
```

To get a list of the merchants that offer the product online, call the API again and set `modules` to ShoppingSources and `insightsToken` to the token found in the similar product image above.  

```
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/images/details?modules=ShoppingSources&insightsToken=ccid_VFnOfyyp*cp_088F3195...&mkt=en-us 
```

The following example shows what the shopping sources insight looks like in the response.  

```json
{
  "_type": "ImageInsights",
  "imageInsightsToken": "ccid_VFnOfyyp*cp_088F3195...",
  "shoppingSources": {
    "offers": [
      {
        "name": "Chocolate brown Informal Zipper Chiffon Knee Length Sequin Bridesmaid Dresses",
        "url": "https:\/\/www.fabrikam.com\/p\/informal-zipper-chiffon-knee-length...",
        "description": "Color:Chocolate Brown;Silhouette: A-line; Neckline: V-neck;...",
        "seller": {
          "name": "fabrikam.com"
        },
        "price": 75.99,
        "priceCurrency": "USD",
        "availability": "InStock",
        "lastUpdated": "2020-08-12T00:00:00.0000000"
      }
    ]
  }
}
```

