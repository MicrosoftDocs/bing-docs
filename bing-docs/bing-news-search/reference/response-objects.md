---
title: Bing News Search APIs v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing News Search APIs may return in the JSON response.
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: reference
author: alekhyasasi
ms.date: 04/05/2022
ms.author: v-apunnamara
---

# News Search APIs v7 response objects

For a list of possible objects, see **In this article** in the right pane.

The top-level object in the response depends on the endpoint you call. If you call `/news/search` or `/news`, the top-level object in the response is the [NewsAnswer](#newsanswer) object; and for `/news/trendingtopics`, it's [TrendingTopicAnswer](#trendingtopics). If the request fails, the top-level object is the [ErrorResponse](#errorresponse) object.

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

Defines a thumbnail image.  
  
> [!NOTE]
> Because URL formats and parameters are subject to change without notice, all image URLs should be used as-is; you should not take dependencies on the URL format or parameters. The exception is those parameters and values discussed by [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).  
  
|Name|Value|Type
|-|-|- 
|provider|A list of owners of the image.|[Organization](#organization) 
|thumbnail|A link to a thumbnail of the image.|[Thumbnail](#thumbnail)  
|url|A URL to the image.|String

## MediaSize  

Defines the size of the media content.  
  
|Name|Value|Type
|-|-|-
|height|The height of the media content, in pixels.|Integer
|width|The width of the media content, in pixels.|Integer 

  
## NewsAnswer  

Defines the top-level object that the response includes when the news request succeeds.  
  
If the service suspects a denial of service attack, the request succeeds (HTTP status code is 200 OK), but the body of the response is empty.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint.|String
|id|An ID that uniquely identifies the news answer. Only Web Search API responses include this field. For information about how to use this field, see [Ranking results](../../bing-web-search/rank-results.md) in the Web Search API guide.|String
|readLink|A URL to request news from News Search API. Include other query parameters as needed. Only Web Search API responses include this field.|String
|<a name="news-relatedtopics"></a>relatedTopics|A list of news articles that are related to the search term.<br/><br/>Only the News Search API includes this field.|[RelatedTopic](#relatedtopic)[]
|<a name="news-sort"></a>sort|A list of options for sorting the news articles. For example, sort by relevance (default) or date. To determine which sort order the request used, see the `isSelected` field.<br/><br/>Only the News Search API includes this field.|[SortValue](#sortvalue)[]
|<a name="news-totalmatches"></a>totalEstimatedMatches|The estimated number of news articles that are relevant to the query. Use this number along with the [count](query-parameters.md#count) and [offset](query-parameters.md#offset) query parameters to page the results.<br/><br/>Only the News Search API includes this field.|Long
|<a name="news-value"></a>value|A list of news articles that are relevant to the query term.<br/><br/>If there are no results to return for the request, the array is empty.|[NewsArticle](#newsarticle)[]  
|webSearchUrl|A URL to the top news stories on Bing. Included only for `/news` requests.|String 

  
## NewsArticle  

Defines a news article.  
  
|Name|Value|Type
|-|-|-
|about|For internal use only.|Object[]
|<a name="newsarticle-category"></a>category|The news category that the article belongs to. For example, Sports. If the news category cannot be determined, the article does not include this field.<br/><br/>For a list of possible categories, see [News Categories by Market](query-parameters.md#news-categories-by-market).<br/><br/>If your request specifies the Sports-Tennis category, the `category` property may contain Sports-Tennis or Sports.|String  
|<a name="newsarticle-clusteredarticles"></a>clusteredArticles|A list of related news articles.|NewsArticle[]
|<a name="newsarticle-contractualrules"></a>contractualRules|A list of rules that you must adhere to if you display the article.<br/><br/>The following contractual rules may apply:<ul><li>[TextAttribution](#textattribution)</li></ul>If the article provides contractual rules, you must abide by them.<br/><br/>**NOTE:** Only articles returned by Web Search API include contractual rules. Articles returned by the News endpoints do not include contractual rules.|Object[]
|<a name="newsarticle-datepublished"></a>datePublished|The date and time that Bing discovered the article. The date is in the format, YYYY-MM-DDTHH:MM:SS.|String
|<a name="newsarticle-description"></a>description|A short description of the news article.|String
|<a name="newsarticle-headline"></a>headline|A Boolean value that indicates whether the news article is a headline. If **true**, the article is a headline.<br/><br/>**NOTE:** The article includes this field only for news categories requests that do not specify the [category](query-parameters.md#category) query parameter.|Boolean
|id|An ID that uniquely identifies this article in the list of articles.<br/><br/>For information about how to use this field, see [Using Ranking to Display Results](../../bing-web-search/rank-results.md) in the Web Search API guide.|String
|<a name="newsarticle-image"></a>image|An image related to the new article.<br/><br/>The `Image` object in this context contains only the `thumbnail` field.|[Image](#image)
|<a name="newsarticle-mentions"></a>mentions|A list of entities (places or persons) mentioned in the article.|[Thing](#thing)[]
|<a name="newsarticle-name"></a>name|The name of the article.<br/><br/>Use this name along with the URL to create a hyperlink that when clicked takes the user to the news article.|String  
|<a name="newsarticle-provider"></a>provider|A list of providers that ran the article.|[Organization](#organization)[]  
|<a name="newsarticle-url"></a>url|A URL to the news article.<br/><br/>Use this URL along with `name` to create a hyperlink that when clicked takes the user to the news article.|String  
|<a name="newsarticle-video"></a>video|A video that's related to the news article.|[Video](#video)

## Organization  

Defines the provider that ran the article.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint.|String
|name|The name of the provider that ran the article.|String
  
## Query  

Defines the search query string.  
  
|Name|Value|Type
|-|-|-
|<a name="query-text"></a>text|A query string that returns the trending topic.|String
  
## RelatedTopic  

Defines a list of news articles that are related to the search query.  
  
|Name|Value|Type
|-|-|-
|<a name="relatedtopic-relatednews"></a>relatedNews|A list of related news articles.|[NewsArticle](#newsarticle)
|<a name="relatedtopic-name"></a>name|The query term that returned the related news articles.|String
|webSearchUrl|A URL that takes the user to the Bing search results for the related query.|String

  
## SortValue  

Defines a sort order to use for the request.  
  
|Name|Value|Type
|-|-|-
|id|An identifier that identifies the articles sort order. The following are the possible values:<ul><li>date &mdash; Sort by date.</li><li>relevance &mdash; Sort by relevance.</li></ul>|String
|isSelected|A Boolean value that determines whether the response used this sort order. If **true**, the response used this sort order.|Boolean  
|name|The display name of the sort order.|String 
|url|A URL that you can use to make the same request using this sort order.|String  


## TextAttribution  

Defines a contractual rule for plain text attribution.  
  
|Name|Value|Type 
|-|-|-
|_type|A type hint, which is set to TextAttribution.|String
|<a name="textattribution-text"></a>text|The attribution text.<br/><br/>Text attribution applies to the news article as a whole and should be displayed immediately following the article's presentation. If there are multiple text or link attribution rules that do not specify a target field, you should concatenate them and display them using a "Data from: " label.|String 
  

## Thing  

Defines an entity that the article mentions.  
  
|Name|Value|Type
|-|-|-
|name|The name of the entity that the article mentions.|String

  
## Thumbnail  

Defines a link to the related image.  
  
|Name|Value|Type
|-|-|-
|<a name="thumbnail-contenturl"></a>contentUrl|The URL to the image.|String
|<a name="thumbnail-height"></a>height|The height of the image in pixels.|Unsigned Short
|<a name="thumbnail-width"></a>width|The width of the image in pixels.|Unsigned Short
  
## Topic  

Defines a trending news topic.  
  
|Name|Value|Type
|-|-|-
|<a name="topic-image"></a>image|A link to a related image.<br/><br/>The `Image` object in this context contains only the `url` and `provider` fields. The `provider` field is an array of [Organization](#organization) objects that identify the image providers.|Image
|<a name="topic-isbreakingnews"></a>isBreakingNews|A Boolean value that indicates whether the topic is considered breaking news. If the topic is considered breaking news, the value is **true**.|Boolean 
|<a name="topic-name"></a>name|The title of the trending topic.|String  
|newsSearchUrl|A URL to the Bing News search results for the search query term (see the `query` field).|String  
|<a name="topic-query"></a>query|A search query term that returns this trending topic.|[Query](#query)  
|webSearchUrl|A URL to the Bing search results for the search query term (see the `query` field).|String
  
## TrendingTopics  

Defines the top-level object that the response includes when the trending topics request succeeds.  
  
|Name|Value|Type
|-|-|-
|<a name="trendingtopic-value"></a>value|A list of trending news topics on Bing.<br/><br/>If there are no results to return for the request, the array is empty.|[Topic](#topic)[]

## Video  
Defines a video that's related to the news article.  
  
> [!NOTE]
> Because the URL format and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters.  
  
|Name|Value|Type
|-|-|-
|<a name="video-allowhttpsembed"></a>allowHttpsEmbed|A Boolean value that determines whether you may embed the video (see the `embedHtml` field) on pages that use the HTTPS protocol.|Boolean  
|<a name="video-embedhtml"></a>embedHtml|An iFrame that lets you embed and run the video in your webpage.|String  
|<a name="video-motion"></a>motionThumbnailUrl|A URL to an animated thumbnail that shows a preview of the video. Typically, you use this URL to play a preview of the video when the user mouses over the thumbnail of the video on your results page.|String 
|<a name="video-name"></a>name|The name of the video.|String
|<a name="video-thumbnail"></a>thumbnail|The width and height of the thumbnail image or motion thumbnail.|[MediaSize](#mediasize)
|<a name="video-thumbnailurl"></a>thumbnailUrl|A URL to a thumbnail image of the video. For information about resizing the image, see [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).|String
