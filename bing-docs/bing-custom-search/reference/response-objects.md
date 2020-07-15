---
title: Bing Custom Web Search API v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Custom Web Search API may return in the JSON response.
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: reference
ms.date: 07/15/2017
ms.author: scottwhi
---

# Custom Web Search API v7 response objects

> [!NOTE]
> To comply with the new EU Copyright Directive in France, the Bing Web, News, Video, Image and all Custom Search APIs must omit some content from certain EU News sources for French users. The removed content may include thumbnail images and videos, video previews, and snippets which accompany search results from these sources. As a consequence, the Bing APIs may serve fewer results with thumbnail images and videos, video previews, and snippets to French users.

For a list of possible objects, see **In this article** in the right pane.

If the request succeeds, the top-level object in the response is the [SearchResponse](#searchresponse) object. And if the request fails, the top-level object in the response is the [ErrorResponse](#errorresponse) object.

  
## Error  

Defines the error that occurred.  
  
|Name|Value|Type 
|-|-|-  
|<a name="error-code"></a>code|The error code that identifies the category of error. For a list of possible codes, see [Error codes](error-codes.md#error-codes).|String
|<a name="error-message"></a>message|A description of the error.|String 
|<a name="error-moredetails"></a>moreDetails|A description that provides additional information about the error.|String
|<a name="error-parameter"></a>parameter|The query parameter in the request that caused the error.|String
|<a name="error-subcode"></a>subCode|The error code that identifies the error. For example, if `code` is InvalidRequest, `subCode` may be ParameterInvalid or ParameterInvalidValue.|String
|<a name="error-value"></a>value|The query parameter's value that was not valid.|String
  
## ErrorResponse  

The top-level object that the response includes when the request fails.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint, which is set to ErrorResponse.|String
|<a name="errors"></a>errors|A list of errors that describe the reasons why the request failed.|[Error](#error)[]  
  

## MetaTag  

Defines a webpage's metadata.  
  
|Name|Value|Type
|-|-|-
|content|The metadata.|String
|name|The name of the metadata.|String


## OpenGraphImage

Defines the location and dimensions of an image relevant to a webpage.

|Name|Value|Type
|-|-|-
|contentUrl|The image's location.|String
|width|The width of the image. May be zero (0).|UInt32
|height|The height of the image. May be zero (0).|UInt32


## Query  

Defines a search query.  
  
|Name|Value|Type
|-|-|-
|<a name="query-displaytext"></a>displayText|The display version of the query term. This version of the query term may contain special characters that highlight the search term found in the query string. The string contains the highlighting characters only if the query enabled hit highlighting (see the [textDecorations](query-parameters.md#textdecorations) query parameter). For details about hit highlighting, see [Hit highlighting](../hit-highlighting).|String
|<a name="query-text"></a>text|The query string. Use this string as the query term in a new search request.|String  

  
## QueryContext  

Defines the query string that Bing used for the request.   
  
|Name|Value|Type 
|-|-|-
|<a name="querycontext-adultintent"></a>adultIntent|A Boolean value that indicates whether the specified query has adult intent. The value is **true** if the query has adult intent.<br/><br/>If **true**, and the request's [safeSearch](query-parameters.md#safesearch) query parameter is set to Strict, the response contains only news results, if applicable.|Boolean
|<a name="querycontext-alterationoverridequery"></a>alterationOverrideQuery|The query string to use to force Bing to use the original string. For example, if the query string is *saling downwind*, the override query string is *+saling downwind*. Remember to encode the query string, which results in *%2Bsaling+downwind*.<br/><br/>The object includes this field only if the original query string contains a spelling mistake.|String 
|<a name="querycontext-alteredquery"></a>alteredQuery|The query string that Bing used to perform the query. Bing uses the altered query string if the original query string contained spelling mistakes. For example, if the query string is *saling downwind*, the altered query string is *sailing downwind*.<br/><br/>The object includes this field only if the original query string contains a spelling mistake.|String 
|askUserForLocation|A Boolean value that indicates whether Bing requires the user's location to provide accurate results. If you specified the user's location by using the [X-MSEdge-ClientIP](headers.md#clientip) and [X-Search-Location](headers.md#location) headers, you can ignore this field.<br/><br/>For location aware queries, such as "today's weather" or "restaurants near me" that need the user's location to provide accurate results, this field is set to **true**.<br/><br/>For location aware queries that include the location (for example, "Seattle weather"), this field is set to **false**. This field is also set to **false** for queries that are not location aware, such as "best sellers."|Boolean
|<a name="querycontext-originalquery"></a>originalQuery|The query string as specified in the request.|String  

  
## SearchResponse  

The response's top-level object for search requests that succeed.  
  
If the service suspects a denial of service attack, the request succeeds (HTTP status code is 200 OK), but the body of the response is empty.  
  
|Name|Value|Type 
|-|-|-
|_type|Type hint, which is set to SearchResponse.|String
|<a name="searchresponse-querycontext"></a>queryContext|The query string that Bing used for the request.|[QueryContext](#querycontext)
|<a name="search-response-webpages"></a>webPages|A list of webpages that are relevant to the search query.|[WebAnswer](#webanswer)
  
  
## WebAnswer  

Defines a list of relevant webpage links.  
  
|Name|Value|Type
|-|-|-
|<a name="totalmatches"></a>totalEstimatedMatches|The estimated number of webpages that are relevant to the query. Use this number along with the [count](query-parameters.md#count) and [offset](query-parameters.md#offset) query parameters to page the results.|Long  
|<a name="webanswer-value"></a>value|A list of webpages that are relevant to the query.|[WebPage](#webpage)[]  
|<a name="webanswer-websearchurl"></a>webSearchUrl|The URL to the Bing search results for the requested webpages.|String
  

## Webpage  

Defines a webpage that is relevant to the query.  
  
|Name|Value|Type
|-|-|-
|<a name="datelastcrawled"></a>dateLastCrawled|The last time that Bing crawled the webpage. The date is in the form, YYYY-MM-DDTHH:MM:SS. For example, 2015-04-13T05:23:39.|String
|<a name="deeplinks"></a>deepLinks|A list of links to related content that Bing found in the website that contains this webpage.<br/><br/>The `Webpage` object in this context includes only the `name`, `url`, `urlPingSuffix`, and `snippet` fields.|[Webpage](#webpage)[]
|<a name="displayurl"></a>displayUrl|The display URL of the webpage. The URL is meant for display purposes only and is not well formed.|String
|id|An ID that uniquely identifies this webpage in the list of web results.<br/><br/>The object includes this field only if the Ranking answer specifies that you mix the webpages with the other search results. Each webpage contains an ID that matches an ID in the Ranking answer. For more information, see [Ranking results](../rank-results.md).|String
|<a name="name"></a>name|The name of the webpage.<br/><br/>Use this name along with `url` to create a hyperlink that when clicked takes the user to the webpage.|String  
|<a name="openGraphImage"></a>openGraphImage|A URL to the image that the webpage owner chose to represent the page content. Included only if available.|[OpenGraphImage](#opengraphimage)
|<a name="searchtags"></a>searchTags|A list of search tags that the webpage owner specified on the webpage. The API returns only indexed search tags.<br/><br/>The `name` field of the `MetaTag` object contains the indexed search tag. Search tags begin with search.* (for example, search.assetId). The `content` field contains the tag's value.|[MetaTag](#metatag)[]
|snippet|A snippet of text from the webpage that describes its contents.|String
|<a name="url"></a>url|The URL to the webpage.<br/><br/>Use this URL along with `name` to create a hyperlink that when clicked takes the user to the webpage.|String 

