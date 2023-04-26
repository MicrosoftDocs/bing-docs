---
title: Bing Custom Web Search API v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Custom Web Search API may return in the JSON response.
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: reference
author: alekhyasasi
ms.date: 04/05/2022
ms.author: v-apunnamara
---

# Custom Web Search API v7 response objects

For a list of possible objects, see **In this article** in the right pane.

If the request succeeds, the top-level object in the response is:

- [SearchResponse](#searchresponse) for Custom Web Search
- [Suggestions](#suggestions) for Custom Autosuggest 
- [Images](#imageanswer) for Custom Image Search
- [Videos](#videoanswer) for Custom Video Search

And if the request fails, the top-level object in the response is the [ErrorResponse](#errorresponse) object.

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


## ImageAnswer  

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
|<a name="query-displaytext"></a>displayText|The display version of the query term. This version of the query term may contain special characters that highlight the search term found in the query string. The string contains the highlighting characters only if the request enabled hit highlighting (see the [textDecorations](query-parameters.md#textdecorations) query parameter). For details about hit highlighting, see [Hit highlighting](../../bing-web-search/hit-highlighting.md).|String
|<a name="query-searchurl"></a>searchUrl|The URL that you use to get the results of the related search. Before using the URL, append query parameters as appropriate.<br/><br/>Use this URL if you're displaying the results in your own user interface. Otherwise, use the URL in `webSearchUrl`.|String
|<a name="query-text"></a>text|The query string. Use this string as the query term in a new search request.|String  
|<a name="query-thumbnail"></a>thumbnail|The URL to a thumbnail of a related image.<br/><br/>The object includes this field only for pivot suggestions and related searches.|[Thumbnail](#thumbnail) 
|<a name="query-websearchurl"></a>webSearchUrl|The URL that takes the user to the Bing search results page for the query.|String

  
## QueryContext  

Defines the query string that Bing used for the request.   
  
|Name|Value|Type 
|-|-|-
|<a name="querycontext-adultintent"></a>adultIntent|A Boolean value that indicates whether the specified query has adult intent. The value is **true** if the query has adult intent.<br/><br/>If **true**, and the request's [safeSearch](query-parameters.md#safesearch) query parameter is set to Strict, the response contains only news results, if applicable.|Boolean
|<a name="querycontext-alterationoverridequery"></a>alterationOverrideQuery|The query string to use to force Bing to use the original string. For example, if the query string is *saling downwind*, the override query string is *+saling downwind*. Remember to encode the query string, which results in *%2Bsaling+downwind*.<br/><br/>The object includes this field only if the original query string contains a spelling mistake.|String 
|<a name="querycontext-alteredquery"></a>alteredQuery|The query string that Bing used to perform the query. Bing uses the altered query string if the original query string contained spelling mistakes. For example, if the query string is *saling downwind*, the altered query string is *sailing downwind*.<br/><br/>The object includes this field only if the original query string contains a spelling mistake.|String 
|askUserForLocation|A Boolean value that indicates whether Bing requires the user's location to provide accurate results. If you specified the user's location by using the [X-MSEdge-ClientIP](headers.md#clientip) and [X-Search-Location](headers.md#location) headers, you can ignore this field.<br/><br/>For location aware queries, such as "today's weather" or "restaurants near me" that need the user's location to provide accurate results, this field is set to **true**.<br/><br/>For location aware queries that include the location (for example, "Seattle weather"), this field is set to **false**. This field is also set to **false** for queries that are not location aware, such as "best sellers."|Boolean
|<a name="querycontext-originalquery"></a>originalQuery|The query string as specified in the request.|String  
  

## SearchAction  

Defines a query search suggestion.  
  
|Name|Value|Type
|-|-|-
|displayText|The suggested query term to display in a user interface. |String
|<a name="searchaction-query"></a>query|The suggested query term.<br/><br/>If the user selects the query term from the list of suggestions, use the term in a Bing API request and display the search results yourself.|String
|<a name="searchaction-searchkind"></a>searchKind|The type of suggestion. The following are the possible values:<ul><li>CustomSearch &mdash; The suggestion is from a non-web search suggestion data source.</li><li>WebSearch &mdash; The suggestion is from a web search suggestion data source.</li></ul>|String
  

## SuggestionGroup  

Defines a group of suggestions of the same type.  
  
|Name|Value|Type
|-|-|-
|<a name="suggestgroup-name"></a>name|The name of the group. The name identifies the type of suggestions that the group contains. For example, web search suggestions. The following are the possible group names:<ul><li>Custom &mdash; The group contains suggestions from a non-web search suggestions data source.</li><li>Web &mdash; The group contains web search suggestions.</li></ul>|String
|<a name="suggestgroup-searchsuggestions"></a>searchSuggestions|A list of up to 8 suggestions. If there are no suggestions, the array is empty.<br/><br/>You must display all suggestions in the order provided. The list is in order of decreasing relevance. The first suggestion is the most relevant and the last suggestion is the least relevant. The size of the list is subject to change.|[SearchAction](#searchaction)[]  
  

## Suggestions  

The top-level object that the response includes when the request succeeds.  
  
If the service suspects a denial of service attack, the request succeeds (HTTP status code is 200 OK). However, the body of the response is empty.  
  
|Name|Value|Type
|-|-|-
|_type|The type hint, which is set to Suggestions.|String
|<a name="suggestions-suggestiongroups"></a>suggestionGroups|A list of suggested query strings grouped by type. For example, web search suggestions.|[SuggestionGroup](#suggestiongroup)[]

  
## SearchResponse  

The response's top-level object for search requests that succeed.  
  
If the service suspects a denial of service attack, the request succeeds (HTTP status code is 200 OK), but the body of the response is empty.  
  
|Name|Value|Type 
|-|-|-
|_type|Type hint, which is set to SearchResponse.|String
|<a name="searchresponse-querycontext"></a>queryContext|The query string that Bing used for the request.|[QueryContext](#querycontext)
|<a name="search-response-images"></a>images|A list of images that are relevant to the search query.|[ImageAnswer](#imageanswer)
|<a name="search-response-videos"></a>videos|A list of videos that are relevant to the search query.|[VideoAnswer](#videoanswer)
|<a name="search-response-webpages"></a>webPages|A list of webpages that are relevant to the search query.|[WebAnswer](#webanswer)
  
  
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
|<a name="video-mainentity"></a>mainEntity|The name of the main entity shown in the video.<br/><br/>The object includes this field only when the `scenario` field in the [VideoAnswer](#videoanswer) object is set to SingleDominantVideo.|[Thing](#thing)
|<a name="video-motionthumbnailurl"></a>motionThumbnailUrl|The URL to an animated thumbnail that shows a preview of the video. Typically, you use this URL to play a preview of the video when the user mouses over the thumbnail image of the video on your results page.|String  
|<a name="video-name"></a>name|The name of the video.|String
|<a name="video-publisher"></a>publisher|A list of the publishers that published the video.|[Publisher[]](#publisher) 
|<a name="video-thumbnail"></a>thumbnail|The width and height of the thumbnail image (see `thumbnailUrl`).|[MediaSize](#mediasize)
|<a name="video-thumbnailurl">thumbnailUrl|The URL to a thumbnail image of the video. For information about resizing the image, see [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).|String
|<a name="video-videoid"></a>videoId|An ID that uniquely identifies this video in the list of videos.|String  
|<a name="video-viewcount"></a>viewCount|The number of times that the video has been watched at the source site.|Integer 
|<a name="video-websearchurl"></a>webSearchUrl|The URL that takes the user to the Bing video search results and plays the video.|String
|width|The width of the video, in pixels.|Integer 

<!--
 You can use the ID in a subsequent request to ensure that this video is the first video returned in the list of videos. To ensure the video is the first video in the list, set the request's [id](query-parameters.md#id) query parameter to this ID.
-->  
  
## VideoAnswer  

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
|id|An ID that uniquely identifies this webpage in the list of web results.<br/><br/>The object includes this field only if the Ranking answer specifies that you mix the webpages with the other search results. Each webpage contains an ID that matches an ID in the Ranking answer. For more information, see [Ranking results](../../bing-web-search/rank-results.md).|String
|<a name="name"></a>name|The name of the webpage.<br/><br/>Use this name along with `url` to create a hyperlink that when clicked takes the user to the webpage.|String  
|<a name="openGraphImage"></a>openGraphImage|A URL to the image that the webpage owner chose to represent the page content. Included only if available.|[OpenGraphImage](#opengraphimage)
|<a name="searchtags"></a>searchTags|A list of search tags that the webpage owner specified on the webpage. The API returns only indexed search tags.<br/><br/>The `name` field of the `MetaTag` object contains the indexed search tag. Search tags begin with search.* (for example, search.assetId). The `content` field contains the tag's value.|[MetaTag](#metatag)[]
|snippet|A snippet of text from the webpage that describes its contents.|String
|<a name="url"></a>url|The URL to the webpage.<br/><br/>Use this URL along with `name` to create a hyperlink that when clicked takes the user to the webpage.|String 

