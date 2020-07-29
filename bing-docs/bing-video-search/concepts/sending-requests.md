---
title: "Send search requests to the Bing Video Search API"
titleSuffix: Bing Search Services
description: This article describes the parameters and attributes of requests sent to the Bing Video Search API, as well as the JSON response object it returns.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Sending search requests to the Bing Video Search API

This article describes the parameters and attributes of requests sent to the Bing Video Search API, as well as the JSON response object it returns. 

[!INCLUDE [bing-video-search-signup-requirements](../../../includes/bing-video-search-signup-requirements.md)]


## Sending a request

To get Video search results, you'd send a GET request to the following endpoint:  
  
```
https://api.bing.microsoft.com/bing/v7.0/videos/search
```
   
The request must use the HTTPS protocol.

We recommend that all requests originate from a server. Distributing the key as part of a client application provides more opportunity for a malicious third party to access it. Making calls from a server also provides a single upgrade point for future versions of the API.

  
The request must specify the [q](../reference/query-parameters.md#query) query parameter, which contains the user's search term. Although it's optional, the request should also specify the [mkt](../reference/query-parameters.md#mkt) query parameter, which identifies the market where you want the results to come from. For a list of optional query parameters such as `pricing`, see [Query Parameters](../reference/query-parameters.md). All query parameter values must be URL encoded.  
  
The request must specify the [Ocp-Apim-Subscription-Key](../reference/headers.md#subscriptionkey) header. Although optional, you are encouraged to also specify the following headers:  
  
-   [User-Agent](../reference/headers.md#useragent)  
-   [X-MSEdge-ClientID](../reference/headers.md#clientid)  
-   [X-Search-ClientIP](../reference/headers.md#clientip)  
-   [X-Search-Location](../reference/headers.md#location)  

The client IP and location headers are important for returning location aware content.  

For a list of all request and response headers, see [Headers](../reference/headers.md).

## Example search request

The following shows a search request that includes all the suggested query parameters and headers. If it's your first time calling any of the Bing APIs, don't include the client ID header. Only include the client ID if you've previously called a Bing API and Bing returned a client ID for the user and device combination. 
  
```  
GET https://api.bing.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

## Example JSON response

The following shows the response to the previous request. The example also shows the Bing-specific response headers.

[!INCLUDE [bing-url-note](../../../includes/bing-url-note.md)]

```json
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D5694...",
    "totalEstimatedMatches" : 1000,
    "value" : [
        {
            "name" : "How to sail - What to Wear for Dinghy Sailing",
            "description" : "An informative video on what to wear when...",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
            "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.DYWCvh...",
            "datePublished" : "2014-03-04T11:51:53",
            "publisher" : [
                {
                    "name" : "Fabrikam"
                }
            ],
            "creator" : {
                "name" : "Marcus Appel"
            },
            "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjHZ--g",
            "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56944...",
            "encodingFormat" : "h264",
            "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjBZ--g",
            "width" : 1280,
            "height" : 720,
            "duration" : "PT2M47S",
            "motionThumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OM.Y6...",
            "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"https:...><\/iframe>",
            "allowHttpsEmbed" : true,
            "viewCount" : 8743,
            "thumbnail" : {
                "width" : 300,
                "height" : 168
            },
            "videoId" : "6DB795E11A6E3CBAAD636DB795E11E3CBAAD63",
            "allowMobileEmbed" : true,
            "isSuperfresh" : false
        },
        . . .
    ],
    "nextOffset" : 0,
    "pivotSuggestions" : [
        {
            "pivot" : "sailing",
            "suggestions" : []
        },
        {
            "pivot" : "dinghies",
            "suggestions" : [
                {
                    "text" : "Sailing Cruising",
                    "displayText" : "Cruising",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF754...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Sailing..."
                    }
                },
                . . .
            ]
        }
    ]
}
```

## Next steps

For details about consuming the response objects, see [Searching the Web for Videos](get-videos.md).

For details about getting insights about a video such as related searches, see [Video Insights](../video-insights.md).  
  
For details about videos that are trending on social media, see [Trending Videos](../trending-videos.md).  
