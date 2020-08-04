---
title: Bing Custom Image Search API v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Custom Image Search API may return in the JSON response.
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-custom-image-search
ms.topic: reference
ms.date: 07/15/2017
ms.author: scottwhi
---

# Custom Image Search API v7 response objects

> [!NOTE]
> To comply with the new EU Copyright Directive in France, the Bing Web, News, Video, Image and all Custom Search APIs must omit some content from certain EU News sources for French users. The removed content may include thumbnail images and videos, video previews, and snippets which accompany search results from these sources. As a consequence, the Bing APIs may serve fewer results with thumbnail images and videos, video previews, and snippets to French users.

For a list of possible objects, see **In this article** in the right pane.

If the request succeeds, the top-level object in the response is the [Images](#images) object. And if the request fails, the top-level object is the [ErrorResponse](#errorresponse) object.

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.


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
  
## Image  

Defines an image that is relevant to the query.  
  
> [!NOTE]
> Because the URL format and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters. The exception is those parameters and values discussed by [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).  
  
|Name|Value|Type
|-|-|-
|<a name="image-accentcolor"></a>accentColor|A three-byte hexadecimal number that represents the color that dominates the image. Use the color as the temporary background in your client until the image is loaded.|String  
|<a name="image-contentsize"></a>contentSize|The image's file size. The format of the string is {size} {units}. For example, 12345 B indicates that the size of the image is 12,345 bytes.|String
|<a name="image-contenturl"></a>contentUrl|A URL to the image on the source website.|String
|<a name="image-datepublished"></a>datePublished|The date and time, in UTC, that Bing discovered the image. The date is in the format, YYYY-MM-DDTHH:MM:SS.|String  
|<a name="image-encodingformat"></a>encodingFormat|The image's mime type (for example, jpeg).|String 
|<a name="image-height"></a>height|The height of the source image, in pixels.|Unsigned Short
|<a name="image-hostpagedisplayurl"></a>hostPageDisplayUrl|The display URL of the webpage that hosts the image.<br/><br/>Use this URL in your user interface to identify the host webpage that contains the image. The URL is not a well-formed and should not be used to access the host webpage. To access the host webpage, use the `hostPageUrl` URL.|String 
|<a name="image-hostpageurl"></a>hostPageUrl|The URL of the webpage that includes the image. This URL and `contentUrl` may be the same URL.|String 
|<a name="image-imageid"></a>imageId|An ID that uniquely identifies this image.|String
|<a name="image-name"></a>name|A title of the image.|String
|<a name="image-thumbnail"></a>thumbnail|The width and height of the thumbnail image (see `thumbnailUrl`).|[MediaSize](#mediasize) 
|<a name="image-thumbnailurl"></a>thumbnailUrl|A URL to a thumbnail of the image. For information about resizing the image, see [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).|String
|webSearchUrl|A URL to the Bing search results for this image.|String  
|<a name="image-width"></a>width|The width of the source image, in pixels.|Unsigned Short 


## Images  

The top-level object that the response includes when an image request succeeds.  
  
|Name|Value|Type
|-|-|-
|_type|A type hint, which is set to Images.|String 
|<a name="nextoffset"></a>nextOffset|The offset value that you set the [offset](query-parameters.md#offset) query parameter to.<br/><br/>If you set `offset` to 0 and `count` to 30 in your first request, and then set `offset` to 30 in your second request, some of the results in the second response may be duplicates of the first response. To prevent duplicates, set `offset` to the value of `nextOffset`.|Integer
|<a name="pivotsuggestions"></a>pivotSuggestions|A list of segments in the original query. For example, if the query was *Red Flowers*, Bing might segment the query into *Red* and *Flowers*.<br/><br/>The Flowers pivot may contain query suggestions such as Red Peonies and Red Daisies, and the Red pivot may contain query suggestions such as Green Flowers and Yellow Flowers.|[Pivot](#pivot)  
|<a name="queryexpansions"></a>queryExpansions|A list of expanded queries that narrows the original query. For example, if the query was *Microsoft Surface*, the expanded queries might be: Microsoft Surface **Pro 3**, Microsoft Surface **RT**, Microsoft Surface **Phone**, and Microsoft Surface **Hub**.|[Query](#query)  
|readLink|A URL that returns this answer. To use the URL, append query parameters as appropriate.<br/><br/>This field is included only in a Web Search API response. Typically, you'd use the URL if you want to query the Image Search API directly.|String 
|<a name="similarterms">similarTerms|A list of terms that are similar in meaning to the user's query term.|[Query](#query)
|<a name="totalestimatedmatches"></a>totalEstimatedMatches|The estimated number of images that are relevant to the query. Use this number along with the [count](query-parameters.md#count) and [offset](query-parameters.md#offset) query parameters to page the results.<br/><br/>Only the Image Search API includes this field.|Long
|<a name="images-value">value|A list of images that are relevant to the query. If there are no results, the array is empty.|[Image](#image)[]
|webSearchUrl|A URL to the Bing search results for the requested images.|String 


## MediaSize  

Defines the size of the media content.  
  
|Name|Value|Type
|-|-|-
|height|The height of the media content, in pixels.|Integer
|width|The width of the media content, in pixels.|Integer 

## Pivot  

Defines the pivot segment.  
  
|Name|Value|Type
|-|-|-
|<a name="pivot-pivot"></a>pivot|The segment from the original query to pivot on.|String  
|<a name="pivot-suggestions"></a>suggestions|A list of suggested query strings for the pivot.|[Query](#query)
  

## Query  

Defines a search query.  
  
|Name|Value|Type
|-|-|-
|<a name="query-displaytext"></a>displayText|The display version of the query term.|String
|<a name="query-searchurl"></a>searchUrl|The URL that you use to get the results of the related search. Before using the URL, append query parameters as appropriate.<br/><br/>Use this URL if you're displaying the results in your own user interface. Otherwise, use the URL in `webSearchUrl`.|String
|<a name="query-text"></a>text|The query string. Use this string as the query term in a new search request.|String  
|<a name="query-thumbnail"></a>thumbnail|The URL to a thumbnail of a related image.<br/><br/>The object includes this field only for pivot suggestions and related searches.|[Thumbnail](#thumbnail) 
|<a name="query-websearchurl"></a>webSearchUrl|The URL that takes the user to the Bing search results page for the query.|String
  
  
## Thumbnail  

Defines the URL to a thumbnail of an image.  
  
|Name|Value|Type
|-|-|-
|url|The URL to a thumbnail of an image.|String
  
