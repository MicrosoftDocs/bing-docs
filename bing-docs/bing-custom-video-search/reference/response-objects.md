---
title: Bing Custom Video Search API v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Custom Video Search API may return in the JSON response.
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-custom-video-search
ms.topic: reference
ms.date: 07/15/2017
ms.author: scottwhi
---

# Custom Video Search API v7 response objects

> [!NOTE]
> To comply with the new EU Copyright Directive in France, the Bing Web, News, Video, Image and all Custom Search APIs must omit some content from certain EU News sources for French users. The removed content may include thumbnail images and videos, video previews, and snippets which accompany search results from these sources. As a consequence, the Bing APIs may serve fewer results with thumbnail images and videos, video previews, and snippets to French users.

For a list of possible objects, see **In this article** in the right pane.

If the request succeeds, the top-level object in the response is the [Videos](#videos) object. And if the request fails, the top-level object is the [ErrorResponse](#errorresponse) object.

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
  
## Publisher  

Defines a publisher or creator.  
  
|Name|Value|Type  
|-|-|-
|name|The name of the publisher or creator.|String 

## Query  

Defines a search query.  
  
|Name|Value|Type
|-|-|-
|<a name="query-displaytext"></a>displayText|The display version of the query term.|String
|<a name="query-searchurl"></a>searchUrl|The URL that you use to get the results of the related search. Before using the URL, append query parameters as appropriate.<br/><br/>Use this URL if you're displaying the results in your own user interface. Otherwise, use the URL in `webSearchUrl`.|String
|<a name="query-text"></a>text|The query string. Use this string as the query term in a new search request.|String  
|<a name="query-thumbnail"></a>thumbnail|The URL to a thumbnail of a related image.<br/><br/>The object includes this field only for pivot suggestions and related searches.|[Thumbnail](#thumbnail) 
|<a name="query-websearchurl"></a>webSearchUrl|The URL that takes the user to the Bing search results page for the query.|String
  
  
## Thing  

Defines the main entity shown in the video.  
  
|Name|Value|Type
|-|-|-
|name|The name of the main entity shown in the video.|String  
  
## Thumbnail  

Defines the URL to a thumbnail of an image.  
  
|Name|Value|Type
|-|-|-
|url|The URL to a thumbnail of an image.|String
  
  
## Video  

Defines a video that is relevant to the query.  
  
> [!NOTE]
> Because the URL format and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters.  
  
|Name|Value|Type 
|-|-|-
|<a name="video-allowhttpsembed"></a>allowHttpsEmbed|A Boolean value that determines whether you may embed the video in the `embedHtml` field on pages that use the HTTPS protocol.|Boolean  
|<a name="video-allowmobileembed"></a>allowMobileEmbed|A Boolean value that determines whether you may embed the video in the `embedHtml` field on a mobile device. If `true`, you may use the HTML on a mobile device.|Boolean 
|<a name="video-creator"></a>creator|The name of the video's creator.<br/><br/>Only Video Search API responses include this field.|[Publisher](#publisher)
|<a name="video-contenturl"></a>contentUrl|The URL to the video on the host website.|String
|<a name="video-datepublished"></a>datePublished|The date and time that Bing discovered the video. The date is in the format, YYYY-MM-DDTHH:MM:SS.|String 
|<a name="video-description"></a>description|A short description of the video.|String 
|<a name="video-duration"></a>duration|The video's duration or length. For example, PT2M50S. For information about the format, see <a href="https://en.wikipedia.org/wiki/ISO_8601#Durations" target="_blank">ISO 8601 Durations on Wikipedia.|String
|<a name="video-embedhtml"></a>embedHtml|An iFrame that lets you embed and run the video on your webpage.|String 
|<a name="video-encodingformat"></a>encodingFormat|The video's mime type (for example, mp4).|String  
|height|The height of the video, in pixels.|Integer
|<a name="video-hostpagedisplayurl"></a>hostPageDisplayUrl|The display URL of the webpage that hosts the video.<br/><br/>Use this URL in your user interface to identify the host webpage that contains the video. The URL is not a well-formed and should not be used to access the host webpage. To access the host webpage, use the URL in `hostPageUrl`.|String
|<a name="video-hostpageurl"></a>hostPageUrl|The URL to the webpage that hosts the video.<br/><br/>This URL and `contentUrl` URL may be the same URL.|String
|id|An ID that uniquely identifies this video in the list of videos.<br/><br/>Only Web Search API responses include this field. For information about how to use this field, see [Ranking results](../../bing-web-search/rank-results.md) in the Web Search API guide.|String 
|<a name="video-isaccessibleforfree"></a>isAccessibleForFree|A Boolean value that indicates whether the video requires payment or a paid subscription to view. If **true**, the video is free to watch. Otherwise, if **false**, a payment or subscription is required.<br/><br/>**NOTE:** If Bing is unable to determine whether payment is required, the object may not include this field.<br/><br/>To ensure that Bing returns only free videos, set the [pricing](query-parameters.md#pricing) query parameter to Free.|Boolean
|<a name="video-issuperfresh"></a>isSuperfresh|A Boolean value that indicates whether the video was recently discovered by Bing. If **true**, the video was recently discovered.<br/><br/>To get videos discovered within the last 24 hours or the last week, use the [freshness](query-parameters.md#freshness) query parameter.|Boolean  
|<a name="video-mainentity"></a>mainEntity|The name of the main entity shown in the video.<br/><br/>The object includes this field only when the `scenario` field in the [Videos](#videos) object is set to SingleDominantVideo.|[Thing](#thing)
|<a name="video-motionthumbnailurl"></a>motionThumbnailUrl|The URL to an animated thumbnail that shows a preview of the video. Typically, you use this URL to play a preview of the video when the user mouses over the thumbnail image of the video on your results page.|String  
|<a name="video-name"></a>name|The name of the video.|String
|<a name="video-publisher"></a>publisher|A list of the publishers that published the video.|[Publisher](#publisher) 
|<a name="video-thumbnail"></a>thumbnail|The width and height of the thumbnail image (see `thumbnailUrl`).|[MediaSize](#mediasize)
|<a name="video-thumbnailurl">thumbnailUrl|The URL to a thumbnail image of the video. For information about resizing the image, see [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).|String
|<a name="video-videoid"></a>videoId|An ID that uniquely identifies this video in the list of videos. You can use the ID in a subsequent request to ensure that this video is the first video returned in the list of videos. To ensure the video is the first video in the list, set the request's [id](query-parameters.md#id) query parameter to this ID.|String  
|<a name="video-viewcount"></a>viewCount|The number of times that the video has been watched at the source site.|Integer 
|<a name="video-websearchurl"></a>webSearchUrl|The URL that takes the user to the Bing video search results and plays the video.|String
|width|The width of the video, in pixels.|Integer 
  
  
## Videos  

The top-level object that the response includes when the Video Search API request succeeds.  
  
If the service suspects a denial of service attack, the request succeeds (HTTP status code is 200 OK), but the body of the response is empty.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint, which is set to Videos.|String
|<a name="videos-nextoffset"></a>nextOffset|The offset value that you set the [offset](query-parameters.md#offset) query parameter to.<br/><br/>If you set `offset` to 0 and `count` to 30 on your first request, and then set `offset` to 30 on your second request, some of the results in the second response may be duplicates of the first response. To prevent duplicates, set `offset` to the value of `nextOffset`.|Integer
|<a name="videos-pivotsuggestions"></a>pivotSuggestions|A list of pivots that segment the original query. For example, if the query was *Cleaning Gutters*, Bing might segment the query into *Cleaning* and *Gutters*.<br/><br/>The Cleaning pivot may contain query suggestions such as Gutter Installation and Gutter Repair, and the Gutters pivot may contain query suggestions such as Roof Cleaning and Window Cleaning.|[Pivot](#pivot)[] 
|<a name="videos-queryexpansion"></a>queryExpansions|A list of expanded queries that narrows the original query. For example, if the query was *Cleaning+Gutters*, the expanded queries might be: Gutter Cleaning **Tools**, Cleaning Gutters **From the Ground**, Gutter Cleaning **Machine**, and **Easy** Gutter Cleaning.|[Query](#query)[]  
|<a name="videos-totalestimatedmatches"></a>totalEstimatedMatches|The estimated number of videos that match the query. Use this number along with the [count](query-parameters.md#count) and [offset](query-parameters.md#offset) query parameters to page the results.<br/><br/>Only Video Search API responses include this field.|Long
|<a name="videos-value"></a>value|The list of videos that are relevant to the user's query.|[Video](#video)[] 
|<a name="videos-websearchurl"></a>webSearchUrl|The URL to the Bing search results for the requested videos.|String 
  
  
