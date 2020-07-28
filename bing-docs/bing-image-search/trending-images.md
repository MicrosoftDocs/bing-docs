---
title: Get trending images with the Bing Image Search API
titleSuffix: Bing Search Services
description: Search for today's trending images from the web with the Bing Image Search API.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Get trending images from the web

To get today's trending images, send the following GET request:  

```
GET https://api.bing.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

For a list of markets that support Trending Images, see [Market codes](reference/market-codes.md#trending-image-api-markets).  


The response contains a [TrendingImages](reference/response-objects.md#trendingimages) object that lists images by category. Use the category's `title` to group the images in your user experience. The categories may change daily.  

```json
{
    "_type" : "TrendingImages",  
    "categories" : [{  
        "title" : "Popular people searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Smith",  
                "displayText" : "Mr. Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=smith&FORM=..."
            },  
            "image" : {  
                "id" : "C3C60AE779A054D5CD80D3CACF0F3",  
                "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.M2532...",  
                "contentUrl" : "http:\/\/www.contoso.com.au\/assets\/Uploads\/smith-SH01.jpg",  
                "thumbnail" : {  
                    "width" : 288,  
                    "height" : 300  
                }  
            }  
        },  
        . . .  
        ]  
    },  
    . . .  
    {  
        "title" : "Popular Halloween searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Halloween costumes for adults",  
                "displayText" : "Halloween costumes for adults",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Halloween+costumes..."
            },  
            "image" : {  
                "id" : "0F3395F2983003F89DCEE711B55D7FA53E4",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.Me429c...",  
                "contentUrl" : "http:\/\/images.domain.com\/products\/8179\/1-1\/adult-squirrel...",  
                "thumbnail" : {  
                    "width" : 336,  
                    "height" : 480  
                }  
            }  
        }]  
    }]  
}  
```  

Each tile contains an image and options for getting related images. To get the related images, you can use the query string in the `text` field when calling the Image Search API and display the related images yourself. Or, you can use the URL in `webSearchUrl` to take the user to Bing's images search results page, which contains the related images.

If you call the Image Search API to get the related images, set the [id](reference/query-parameters.md#id) query parameter to the ID in the `id` field. Specifying the ID ensures that the response contains the image (it is the first image in the response) and its related images. Also, set the [q](reference/query-parameters.md#query) query parameter to the text in the `query` object's `text` field.

The following example shows how to use the image ID to get related images of Mr. Smith in the preceding Trending Images API response.

```  
GET https://api.bing.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  
