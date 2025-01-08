---
title: Bing Video Search APIs v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Video Search APIs may return in the JSON response.
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-video-search
ms.topic: reference
author: alekhyasasi
ms.date: 07/11/2023
---

# Video Search APIs v7 response objects

For a list of possible objects, see **In this article** in the right pane.

The top-level object in the response depends on the endpoint you call. If you call `/videos/search`, the top-level object in the response is the [VideosAnswer](#videosanswer) object; for `/videos/details`, it's [VideoDetails](#videodetails); and for `/videos/trending`, it's [TrendingVideos](#trendingvideos). If the request fails, the top-level object is the [ErrorResponse](#errorresponse) object.

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.

## Category  

Defines the category of trending videos.  
  
|Name|Value|Type
|-|-|-
|<a name="category-subcategories"></a>subcategories|A list of subcategories. For example, Top Music Videos.|[Subcategory](#subcategory)[]
|<a name="category-title"></a>title|The name of the video category. For example, Music Videos.|String  

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

Defines a thumbnail image.  
  
> [!NOTE]
> Because URL formats and parameters are subject to change without notice, all image URLs should be used as-is; you should not take dependencies on the URL format or parameters. The exception is those parameters and values discussed by [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).  
  
|Name|Value|Type
|-|-|-
|<a name="image-contenturl"></a>contentUrl|The URL to the image on the source website.|String  
|<a name="image-description"></a>description|An attribution.|String
|<a name="image-headline"></a>headline|A description of the video.|String
|<a name="image-thumbnailurl"></a>thumbnailUrl|The URL to a thumbnail of the image. For information about resizing the image, see [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).|String

## MediaSize  

Defines the size of the media content.  
  
|Name|Value|Type
|-|-|-
|height|The height of the media content in pixels.|Integer
|width|The width of the media content in pixels.|Integer

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
|<a name="query-displaytext"></a>displayText|The display version of the query term. This version of the query term may contain special characters that highlight the search term found in the query string. The string contains the highlighting characters only if the query enabled hit highlighting (see the [textDecorations](query-parameters.md#textdecorations) query parameter). For details about hit highlighting, see [Hit highlighting](../../bing-web-search/hit-highlighting.md).|String
|<a name="query-searchlink"></a>searchLink|The Video Search API URL that you use to get video search results. The URL includes the *q* query parameter that's set the search string in `text`. Before using the URL, append other query parameters as appropriate.<br/><br/>Use this URL if you're displaying the results in your own user interface. Otherwise, use the URL in `webSearchUrl`.|String
|<a name="query-text"></a>text|The query string. Use this string as the query term in a new search request.|String  
|<a name="query-thumbnail"></a>thumbnail|A URL to a thumbnail image that represents the search string.|[Thumbnail](#thumbnail)
|<a name="query-websearchurl"></a>webSearchUrl|The URL that takes the user to the Bing search results page for the query.|String

## QueryContext  

Defines the query string that Bing used for the request.
  
|Name|Value|Type
|-|-|-
|<a name="adultintent"></a>adultIntent|A Boolean value that indicates whether the specified query has adult intent. The value is **true** if the query has adult intent.<br/><br/>If **true**, and the request's [safeSearch](query-parameters.md#safesearch) query parameter is set to Strict, the response contains only news results, if applicable.|Boolean
|<a name="alterationoverridequery"></a>alterationOverrideQuery|The query string to use to force Bing to use the original string. For example, if the query string is *saling downwind*, the override query string is *+saling downwind*. Remember to encode the query string, which results in *%2Bsaling+downwind*.<br/><br/>The object includes this field only if the original query string contains a spelling mistake.|String
|<a name="alteredquery"></a>alteredQuery|The query string that Bing used to perform the query. Bing uses the altered query string if the original query string contained spelling mistakes. For example, if the query string is *saling downwind*, the altered query string is *sailing downwind*.<br/><br/>The object includes this field only if the original query string contains a spelling mistake.|String
|askUserForLocation|A Boolean value that indicates whether Bing requires the user's location to provide accurate results. If you specified the user's location by using the [X-MSEdge-ClientIP](headers.md#clientip) and [X-Search-Location](headers.md#location) headers, you can ignore this field.<br/><br/>For location aware queries, such as "today's weather" or "restaurants near me" that need the user's location to provide accurate results, this field is set to **true**.<br/><br/>For location aware queries that include the location (for example, "Seattle weather"), this field is set to **false**. This field is also set to **false** for queries that are not location aware, such as "best sellers."|Boolean
|<a name="originalquery"></a>originalQuery|The query string as specified in the request.|String  

## Subcategory  

Defines a subcategory of videos.  
  
|Name|Value|Type
|-|-|-
|<a name="subcategory-tiles"></a>tiles|A list of videos that are trending in the subcategory. Each tile contains a thumbnail image of the video and a Bing query that returns the video and other related videos.|[Tile](#tile)[]  
|<a name="subcategory-title"></a>title|The name of the subcategory. For example, This Week's Viral Videos.|String

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
  
## Tile  

Defines a video tile.  
  
|Name|Value|Type
|-|-|-
|<a name="tile-image"></a>image|The URL to a thumbnail image of the video.|[Image](#image)  
|<a name="tile-query"></a>query|A query that returns a Bing search results page with videos of the subject. For example, if the category is Top Music Videos, the query returns top music videos.|[Query](#query)  
  
## TrendingVideos  

The top-level object that the response includes when a Trending Videos API request succeeds.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint, which is set to TrendingVideos.|String  
|<a name="trending-bannertiles"></a>bannerTiles|A list of the most popular trending videos.|[Tile](#tile)[]
|<a name="trending-categories"></a>categories|A list of categorized videos. For example, music videos and viral videos.|[Category](#category)[]
  
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
|<a name="video-hostpagedisplayurl"></a>hostPageDisplayUrl|The display URL of the webpage that hosts the video.<br/><br/>Use this URL in your user interface to identify the host webpage that contains the video. The URL is not well-formed and should not be used to access the host webpage. To access the host webpage, use the URL in `hostPageUrl`.|String
|<a name="video-hostpageurl"></a>hostPageUrl|The URL to the webpage that hosts the video.<br/><br/>This URL and `contentUrl` URL may be the same URL.|String
|id|An ID that uniquely identifies this video in the list of videos.<br/><br/>Only Web Search API responses include this field. For information about how to use this field, see [Ranking results](../../bing-web-search/rank-results.md) in the Web Search API guide.|String
|<a name="video-isaccessibleforfree"></a>isAccessibleForFree|A Boolean value that indicates whether the video requires payment or a paid subscription to view. If **true**, the video is free to watch. Otherwise, if **false**, a payment or subscription is required.<br/><br/>**NOTE:** If Bing is unable to determine whether payment is required, the object may not include this field.<br/><br/>To ensure that Bing returns only free videos, set the [pricing](query-parameters.md#pricing) query parameter to Free.|Boolean
|<a name="video-issuperfresh"></a>isSuperfresh|A Boolean value that indicates whether the video was recently discovered by Bing. If **true**, the video was recently discovered.<br/><br/>To get videos discovered within the last 24 hours or the last week, use the [freshness](query-parameters.md#freshness) query parameter.|Boolean  
|<a name="video-mainentity"></a>mainEntity|The name of the main entity shown in the video.<br/><br/>The object includes this field only when the `scenario` field in the [VideosAnswer](#videosanswer) object is set to SingleDominantVideo.|[Thing](#thing)
|<a name="video-motionthumbnailurl"></a>motionThumbnailUrl|The URL to an animated thumbnail that shows a preview of the video. Typically, you use this URL to play a preview of the video when the user mouses over the thumbnail image of the video on your results page.|String  
|<a name="video-name"></a>name|The name of the video.|String
|<a name="video-publisher"></a>publisher|A list of the publishers that published the video.|[Publisher](#publisher)
|<a name="video-thumbnail"></a>thumbnail|The width and height of the thumbnail image (see `thumbnailUrl`).|[MediaSize](#mediasize)
|<a name="video-thumbnailurl">thumbnailUrl|The URL to a thumbnail image of the video. For information about resizing the image, see [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).|String
|<a name="video-videoid"></a>videoId|An ID that uniquely identifies this video in the list of videos. You can use the ID in a subsequent request to ensure that this video is the first video returned in the list of videos. To ensure the video is the first video in the list, set the request's [id](query-parameters.md#id) query parameter to this ID.|String  
|<a name="video-viewcount"></a>viewCount|The number of times that the video has been watched at the source site.|Integer
|<a name="video-websearchurl"></a>webSearchUrl|The URL that takes the user to the Bing video search results and plays the video.|String
|width|The width of the video in pixels.|Integer
  
## VideoDetails  

The top-level object that the response includes when a Video Details API request succeeds.  
  
The modules query parameter affects the fields that Bing includes in the response. If you set `modules` to RelatedVideos, then this object includes only the `relatedVideos` field.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint, which is set to Api.VideoDetails.VideoDetails.|String  
|<a name="videodetails-relatedvideos"></a>relatedVideos|A list of videos that are similar to the specified video.[VideosModule](#videosmodule)
|<a name="videodetails-videoresults"></a>videoResult|The original video that you requested insights of (this is the video that you set the [id](query-parameters.md#id) query parameter to in your insights request).|[Video](#video)
  
## VideosAnswer  

The top-level object that the response includes when the Video Search API request succeeds.  
  
If the service suspects a denial of service attack, the request succeeds (HTTP status code is 200 OK), but the body of the response is empty.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint, which is set to Videos.|String
|currentOffset|The offset that represents where the first video in `value` is relative to all videos that Bing might return for this query. Also see `nextOffset`.
|id|An ID that uniquely identifies the video answer. Only Web Search API responses include this field. For information about how to use this field, see [Ranking results](../../bing-web-search/rank-results.md) in the Web Search API guide.|String
|<a name="video-isfamilyfriendly"></a>isFamilyFriendly|A Boolean value that determines whether one or more of the videos contain adult content. If none of the videos contain adult content, `isFamilyFriendly` is set to **true**. Otherwise, if one or more of the videos contain adult content, `isFamilyFriendly` is set to **false**.<br/><br/>If **false**, the thumbnail images of the videos are pixelated (fuzzy).<br/><br/>**NOTE:** Only Web Search API responses include this field (Video Search API responses do not include this field).|Boolean
|<a name="videos-nextoffset"></a>nextOffset|The offset value that you set the [offset](query-parameters.md#offset) query parameter to.<br/><br/>If you set `offset` to 0 and `count` to 30 on your first request, and then set `offset` to 30 on your second request, some of the results in the second response may be duplicates of the first response. To prevent duplicates, set `offset` to the value of `nextOffset`.<br/><br/>Only Video Search API responses include this field.|Integer
|<a name="videos-pivotsuggestions"></a>pivotSuggestions|A list of pivots that segment the original query. For example, if the query was *Cleaning Gutters*, Bing might segment the query into *Cleaning* and *Gutters*.<br/><br/>The Cleaning pivot may contain query suggestions such as Gutter Installation and Gutter Repair, and the Gutters pivot may contain query suggestions such as Roof Cleaning and Window Cleaning.<br/><br/>Only Video Search API responses include this field.|[Pivot](#pivot)[]
|<a name="querycontext"></a>queryContext|The query string that Bing used for the request.|[QueryContext](#querycontext)
|readLink|A URL that you'd use to request videos from Video Search API using the search query string used for this response.|String
|relatedSearches|A list of related search queries made by others.|[Query](#query)[]
|<a name="videos-scenario"></a>scenario|The scenario that reflects the query's intent. The following are the possible values:<ul><li>List &mdash; For scenarios where there's more than one video that matches the user's intent.<br/><br/></li><li>SingleDominantVideo &mdash; For scenarios where there's a single music video that matches the user's request. This scenario is set only for music videos.</li></ul>Only Web Search API responses include this field|String  
|<a name="videos-totalestimatedmatches"></a>totalEstimatedMatches|The estimated number of videos that match the query. Use this number along with the [count](query-parameters.md#count) and [offset](query-parameters.md#offset) query parameters to page the results.<br/><br/>Only Video Search API responses include this field.|Long
|<a name="videos-value"></a>value|The list of videos that are relevant to the user's query.|[Video](#video)[]
|<a name="videos-websearchurl"></a>webSearchUrl|The URL to the Bing search results for the requested videos.|String
  
## VideosModule  

Defines a list of videos.  
  
|Name|Value|Type
|-|-|-
|value|A list of videos.|[Video](#video)[]
  