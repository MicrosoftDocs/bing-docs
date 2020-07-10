---
title: Bing Web Search API v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Web Search API may return in the JSON response.
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: reference
ms.date: 07/15/2017
ms.author: scottwhi
---

# Response objects

> [!NOTE]
> To comply with the new EU Copyright Directive in France, the Bing Web, News, Video, Image and all Custom Search APIs must omit some content from certain EU News sources for French users. The removed content may include thumbnail images and videos, video previews, and snippets which accompany search results from these sources. As a consequence, the Bing APIs may serve fewer results with thumbnail images and videos, video previews, and snippets to French users.

The following are the JSON response objects that the response may include. These objects are specific to the web answer. For details about the JSON objects for other answer types that the search results may include, see the API-specific reference documentation. For example, if the search results contain the images and news answers, see the <a href="https://docs.microsoft.com/bing/bing-image-search/reference/response-objects" target="_blank">Image Search API reference</a> and <a href="https://docs.microsoft.com/bing/bing-news-search/reference/response-objects" target="_blank">News Search API reference</a>.

If the request succeeds, the top-level object in the response is the [SearchResponse](#searchresponse) object. And if the request fails, the top-level object in the response is the [ErrorResponse](#errorresponse) object.

For a list of possible objects, see **In this article** in the right pane.


## Entity  

Defines an entity such as a person, place, or thing.  
  
|Name|Value|Type
|-|-|-
|bingId|An ID that uniquely identifies this entity.|String  
|contractualRules|A list of rules that you must adhere to if you display the entity. For example, the rules may govern attribution of the entity's description.<br/><br/>The following contractual rules may apply:<ul><li>[LicenseAttribution](#licenseattribution)</li><li>[LinkAttribution](#linkattribution)</li><li>[MediaAttribution](#mediaattribution)</li><li>[TextAttribution](#textattribution)</li></ul>Not all entities include rules. If the entity provides contractual rules, you must abide by them. For more information about using contractual rules, see [Attributing Data](/bing/bing-entities-search/concepts/data-attribution).|Object[]
|description|A short description of the entity.|String  
|entityPresentationInfo|Additional information about the entity such as hints that you can use to determine the entity's type. To determine the entity's type, use the `entityScenario` and `entityTypeHint` fields. For example, the fields help you determine whether the entity is a dominant or disambiguation entity and whether it's a person or movie.<br/><br/>The entity is a dominant entity if Bing believes that only one entity satisfies the request. If multiple entities could satisfy the request, the entity is a disambiguation entity and the user needs to select the entity they're interested in.|[EntityPresentationInfo](#entitypresentationinfo)  
|image|An image of the entity.|[Image](#image)
|name|The entity's name.|String
|webSearchUrl|The URL that takes the user to the Bing search results page for this entity.|String
  
## EntityAnswer  

Defines an entity answer.
  
|Name|Value|Type
|-|-|-  
|queryScenario|The supported query scenario. This field is set to DominantEntity or DisambiguationItem. The field is set to DominantEntity if Bing determines that only a single entity satisfies the request. For example, a book, movie, person, or attraction.<br/><br/>If multiple entities could satisfy the request, the field is set to DisambiguationItem. For example, if the request uses the generic title of a movie franchise, the entity's type would likely be DisambiguationItem. But, if the request specifies a specific title from the franchise, the entity's type would likely be DominantEntity.|String
|value|A list of entities.|[Entity](#entity)[]   

## EntityPresentationInfo  

Defines additional information about an entity such as type hints. 
  
|Name|Value|Type 
|-|-|-  
|entityScenario|The supported scenario.|String
|entityTypeDisplayHint|A display version of the entity hint. For example, if  `entityTypeHints` is Artist, this field may be set to *American Singer*.|String 
|entityTypeHint|A list of hints that indicate the entity's type. The list could contain a single hint such as Movie or a list of hints such as Place, LocalBusiness, Restaurant. Each successive hint in the array narrows the entity's type.<br/><br/>For a list of possible types, see [Entity Types](entity-types.md). If the object does not include this field, Generic is assumed.|String[]  

## Computation  

Defines an expression and its answer.  
  
|Element|Description|Type
|-|-|-
|<a name="computation-expression"></a>expression|The math or conversion expression.<br/><br/>If the query contains a request to convert units of measure (for example, meters to feet), this field contains the *from* units and `value` contains the *to* units.<br/><br/>If the query contains a mathematical expression such as 2+2, this field contains the expression and `value` contains the answer.<br/><br/>Note that mathematical expressions may be normalized. For example, if the query was sqrt(4^2+8^2), the normalized expression may be sqrt((4^2)+(8^2)).<br/><br/>If the user's query is a math question and the [textDecorations](query-parameters.md#textdecorations) query parameter is set to **true**, the expression string may include formatting markers. For example, if the user's query is *log(2)*, the normalized expression includes the subscript markers. For more information, see [Hit highlighting](../hit-highlighting.md).|String
|<a name="computation-value"></a>value|The expression's answer.|String  
  
## Error  

Defines the error that occurred.  
  
|Element|Description|Type 
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
  
## Identifiable  

Defines the identity of a resource.  
  
|Name|Value|Type 
|-|-|-  
|id|An identifier.|String  

## Image  

Defines an image.  
  
> [!NOTE]
> Because URL formats and parameters are subject to change without notice, all image URLs should be used as-is; you should not take dependencies on the URL format or parameters. The exception is those parameters and values discussed by [Resize and crop thumbnail images](../resize-and-crop-thumbnails.md).  
  
|Name|Value|Type
|-|-|- 
|height|The height of the source image, in pixels.|Unsigned Short 
|hostPageUrl|The URL of the webpage that includes the image.<br/><br/>This URL and `contentUrl` may be the same URL.|String
|name|An optional text string that contains random information about the image.|String
|provider|The source of the image. The array will contain a single item.<br/><br/> You must attribute the image to the provider. For example, you may display the provider's name as the cursor hovers over the image or make the image a click-through link to the provider's website where the image is found.<br/><br/>If the image object is part of a larger object, and the larger object contains a contractual rule that applies to this object, you must use the contractual rule for attribution instead of this field.|[Organization](#organization)[]  
|thumbnailUrl|The URL to a thumbnail of the image. For information about resizing the image, see [Resize and crop thumbnail images](../resize-and-crop-thumbnails.md).|String
|width|The width of the source image, in pixels.|Unsigned Short 

## LicenseAttribution  

Defines a contractual rule for license attribution.  
  
|Name|Value|Type  
|-|-|-
|_type|A type hint, which is set to LicenseAttribution.|String
|license|The license under which the content may be used.|License
|licenseNotice|The license to display next to the targeted field. For example, "Text under CC-BY-SA license".<br/><br/>Use the license's name and URL in the `license` field to create a hyperlink to the website that describes the details of the license. Then, replace the license name in the `licenseNotice` string (for example, CC-BY-SA) with the hyperlink you just created.|String
|mustBeCloseToContent|A Boolean value that determines whether the contents of the rule must be placed in close proximity to the field that the rule applies to. If **true**, the contents must be placed in close proximity. If **false**, or this field does not exist, the contents may be placed at the caller's discretion.|Boolean
|targetPropertyName|The name of the field that the rule applies to.|String
  
## LinkAttribution  

Defines a contractual rule for link attribution.  
  
|Name|Value|Type
|-|-|-
|_type|A type hint, which is set to LinkAttribution.|String
|mustBeCloseToContent|A Boolean value that determines whether the contents of the rule must be placed in close proximity to the field that the rule applies to. If **true**, the contents must be placed in close proximity. If **false**, or this field does not exist, the contents may be placed at the caller's discretion.|Boolean 
|targetPropertyName|The name of the field that the rule applies to.<br/><br/>If a target is not specified, the attribution applies to the entity as a whole and should be displayed immediately following the entity presentation. If there are multiple text and link attribution rules that do not specify a target, you should concatenate them and display them using a "Data from: " label. For example, “Data from <provider name1\> &#124; <provider name2\>".|String 
|text|The attribution text.|String 
|url|The URL to the provider's website. Use `text` and URL to create of hyperlink.|String

## MediaAttribution  

Defines a contractual rule for media attribution.  
  
|Name|Value|Type 
|-|-|-  
|_type|A type hint, which is set to MediaAttribution.|String
|mustBeCloseToContent|A Boolean value that determines whether the contents of the rule must be placed in close proximity to the field that the rule applies to. If **true**, the contents must be placed in close proximity. If **false**, or this field does not exist, the contents may be placed at the caller's discretion.|Boolean 
|targetPropertyName|The name of the field that the rule applies to.|String 
|url|The URL that you use to create of hyperlink of the media content. For example, if the target is an image, you would use the URL to make the image clickable.|String  

## MetaTag  

Defines a webpage's metadata.  
  
|Name|Value|Type
|-|-|-
|content|The metadata.|String
|name|The name of the metadata.|String

## Organization  

Defines a publisher.  
  
Note that a publisher may provide their name or their website or both.  
  
|Name|Value|Type
|-|-|-
|name|The publisher's name.|String
|url|The URL to the publisher's website.<br/><br/>Note that the publisher may not provide a website.|String

## Query  

Defines a search query.  
  
The [SpellSuggestions](#spellsuggestions) object uses this object to suggest a query string that likely represents the user's intent. It's also used by [RelatedSearchAnswer](#relatedsearchanswer) to return a related query that other users have made.  
  
|Name|Value|Type
|-|-|-
|<a name="query-displaytext"></a>displayText|The display version of the query term. This version of the query term may contain special characters that highlight the search term found in the query string. The string contains the highlighting characters only if the query enabled hit highlighting (see the [textDecorations](query-parameters.md#textdecorations) query parameter). For details about hit highlighting, see [Hit highlighting](../hit-highlighting).|String
|<a name="query-text"></a>text|The query string. Use this string as the query term in a new search request.|String  
|<a name="query-websearchurl"></a>webSearchUrl|The URL that takes the user to the Bing search results page for the query.<br/><br/>Only related search results include this field.|String
  
## QueryContext  

Defines the query string that Bing used for the request.   
  
|Element|Description|Type 
|-|-|-
|<a name="querycontext-adultintent"></a>adultIntent|A Boolean value that indicates whether the specified query has adult intent. The value is **true** if the query has adult intent.<br/><br/>If **true**, and the request's [safeSearch](query-parameters.md#safesearch) query parameter is set to Strict, the response contains only news results, if applicable.|Boolean
|<a name="querycontext-alterationoverridequery"></a>alterationOverrideQuery|The query string to use to force Bing to use the original string. For example, if the query string is *saling downwind*, the override query string is *+saling downwind*. Remember to encode the query string, which results in *%2Bsaling+downwind*.<br/><br/>The object includes this field only if the original query string contains a spelling mistake.|String 
|<a name="querycontext-alteredquery"></a>alteredQuery|The query string that Bing used to perform the query. Bing uses the altered query string if the original query string contained spelling mistakes. For example, if the query string is *saling downwind*, the altered query string is *sailing downwind*.<br/><br/>The object includes this field only if the original query string contains a spelling mistake.|String 
|askUserForLocation|A Boolean value that indicates whether Bing requires the user's location to provide accurate results. If you specified the user's location by using the [X-MSEdge-ClientIP](headers.md#clientip) and [X-Search-Location](headers.md#location) headers, you can ignore this field.<br/><br/>For location aware queries, such as "today's weather" or "restaurants near me" that need the user's location to provide accurate results, this field is set to **true**.<br/><br/>For location aware queries that include the location (for example, "Seattle weather"), this field is set to **false**. This field is also set to **false** for queries that are not location aware, such as "best sellers."|Boolean
|<a name="querycontext-originalquery"></a>originalQuery|The query string as specified in the request.|String  
  
## RankingGroup  

Defines a search results group, such as mainline.  
  
|Name|Value|Type
|-|-|-
|<a name="rankinggroup-items"></a>items|A list of search result items to display in the group.|[RankingItem](#rankingitem)[]
  
## RankingItem  

Defines a search result item to display. For more information about how to use the IDs, see [Ranking results](../rank-results.md).  
  
|Name|Value|Type
|-|-|-
|<a name="rankingitem-answertype"></a>answerType|The answer that contains the item to display. For example, News.<br/><br/>Use the type to find the answer in the [SearchResponse](#searchresponse) object. The type is the name of a field in the `SearchResponse` object.|String
|<a name="rankingitem-resultindex"></a>resultIndex|A zero-based index of the item in the answer.<br/><br/>If the item does not include this field, display all items in the answer. For example, display all news articles in the News answer.|Integer
|<a name="rankingitem-value"></a>value|The ID that identifies either an answer to display or an item of an answer to display. If the ID identifies an answer, display all items of the answer.|[Identifiable](#identifiable)
  
## RankingResponse  

Defines where on the search results page content should be placed and in what order.  
  
|Name|Value|Type
|-|-|-
|<a name="ranking-mainline"></a>mainline|The search results to display in the mainline section of the search results page.|[RankingGroup](#rankinggroup)  
|<a name="ranking-pole"></a>pole|The search results that should be afforded the most visible treatment (for example, displayed above the mainline and sidebar).|[RankingGroup](#rankinggroup) 
|<a name="ranking-sidebar"></a>sidebar|The search results to display in the sidebar section of the search results page.|[RankingGroup](#rankinggroup) 
  
## RelatedSearchAnswer  

Defines a list of related queries made by others.  
  
|Name|Value|Type
|-|-|- 
|id|An ID that uniquely identifies the related search answer.<br/><br/>The object includes this field only if the Ranking answer specifies that you should display all related searches in a group. For more information about how to use the ID, see [Ranking results](.../rank-results.md).|String
|<a name="relatedsearch-value"></a>value|A list of related queries that were made by others.|[Query](#query)[]  
  
## SearchResponse  

The response's top-level object for search requests that succeed.  
  
By default, the Search API includes all relevant answers unless:  
  
-   The query specifies the [responseFilter](query-parameters.md#responsefilter) query parameter to limit the answers it returns 
-   One or more of the search components does not return results (for example, no news results are relevant to the query)  
-   The subscription key does not have access to the search component.  
  
If the service suspects a denial of service attack, the request succeeds (HTTP status code is 200 OK), but the body of the response is empty.  
  
|Name|Value|Type 
|-|-|-
|_type|Type hint, which is set to SearchResponse.|String
|<a name="searchresponse-computation"></a>computation|The answer to a math expression or unit conversion expression.|[Computation](#computation)
|<a name="searchresponse-entities"></a>entities|A list of entities that are relevant to the search query.|[EntityAnswer](#entityanswer)
|<a name="searchresponse-images"></a>images|A list of images that are relevant to the search query.|[Images](bing-image-search/reference/response-objects.md#images)
|<a name="searchresponse-news"></a>news|A list of news articles that are relevant to the search query.|[News](bing-news-search/reference/response-objects.md#news) 
|<a name="searchresponse-querycontext"></a>queryContext|The query string that Bing used for the request.|[QueryContext](#querycontext)
|<a name="searchresponse-ranking"></a>rankingResponse|The order that Bing suggests that you display the search results in.|[RankingResponse](#rankingresponse)
|<a name="searchresponse-relatedsearches"></a>relatedSearches|A list of related queries made by others.|[RelatedSearchAnswer](#relatedsearchanswer)
|<a name="searchresponse-spellsuggestions"></a>spellSuggestions|The query string that likely represents the user's intent.|[SpellSuggestions](#spellsuggestions)  
|<a name="searchresponse-timezone"></a>timeZone|The date and time of one or more geographic locations.|[TimeZone](#timezone)
|<a name="searchresponse-videos"></a>videos|A list of videos that are relevant to the search query.|[Videos](bing-video-search/reference/response-objects.md#videos)
|<a name="search-response-webpages"></a>webPages|A list of webpages that are relevant to the search query.|[WebAnswer](#webanswer)
  
## SpellSuggestions  

Defines a suggested query string that likely represents the user's intent.  
  
The search results include this response if Bing determines that the user may have intended to search for something different. For example, if the user searches for *alon brown*, Bing may determine that the user likely intended to search for *Alton Brown* instead (based on past searches by others of *alon brown*).  
  
|Name|Value|Type
|-|-|-  
|id|An ID that uniquely identifies the spelling suggestion answer.<br><br>You use this field when you use [ranking response](#rankingresponse) to display the spelling suggestions. For more information about how to use the ID, see [Ranking results](../rank-results.md).|String
|<a name="spell-value"></a>value|A list of suggested query strings that may represent the user's intention.<br><br>The list contains only one `Query` object.|[Query](#query)[]

## TextAttribution  

Defines a contractual rule for plain text attribution.  
  
|Name|Value|Type 
|-|-|- 
|_type|A type hint, which is set to TextAttribution.|String
|text|The attribution text.<br/><br/>Text attribution applies to the entity as a whole and should be displayed immediately following the entity presentation. If there are multiple text or link attribution rules that do not specify a target, you should concatenate them and display them using a "Data from: " label.|String    

## TimeZone  

Defines the date and time of one or more geographic locations.  
  
|Name|Value|Type
|-|-|-
|<a name="othercitytimes"></a>otherCityTimes|A list of dates and times of nearby time zones.|[TimeZoneInformation](#timezoneinformation)[]
|<a name="primarycitytime"></a>primaryCityTime|The data and time, in UTC, of the geographic location specified in the query.<br/><br/>If the query specified a specific geographic location (for example, a city), this object contains the name of the geographic location and the current date and time of the location, in UTC.<br/><br/>If the query specified a general geographic location, such as a state or country, this object contains the date and time of the primary city or state found within the specified state or country. If the location contains additional time zones, the `otherCityTimes` field contains the date and time of cities or states located in the other time zones.|[TimeZoneInformation](#timezoneinformation)
  
## TimeZoneInformation  

Defines a date and time for a geographical location.  
  
|Name|Value|Type
|-|-|-
|<a name="tzinfo-location"></a>location|The name of the geographical location.<br/><br/>For example, County; City; City, State; City, State, Country; or Time Zone.|String  
|<a name="tzinfo-time"></a>time|The date and time specified in the form, YYYY-MM-DDThh;mm:ss.ssssssZ.|String 
|<a name="tzinfo-utcoffset"></a>utcOffset|The offset from UTC. For example, UTC-7.|String
  
## WebAnswer  

Defines a list of relevant webpage links.  
  
|Name|Value|Type
|-|-|-
|id|An ID that uniquely identifies the web answer.<br/><br/>The object includes this field only if the Ranking answer suggests that you display all web results in a group. For more information about how to use the ID, see [Ranking results](../rank-results.md).|String
|<a name="someresultsremoved"></a>someResultsRemoved|A Boolean value that indicates whether the response excluded some results from the answer. If Bing excluded some results, the value is **true**.|Boolean
|<a name="totalmatches"></a>totalEstimatedMatches|The estimated number of webpages that are relevant to the query. Use this number along with the [count](query-parameters.md#count) and [offset](query-parameters.md#offset) query parameters to page the results.|Long  
|<a name="webanswer-value"></a>value|A list of webpages that are relevant to the query.|[WebPage](#webpage)[]  
|<a name="webanswer-websearchurl"></a>webSearchUrl|The URL to the Bing search results for the requested webpages.|String
  
## Webpage  

Defines a webpage that is relevant to the query.  
  
|Name|Value|Type
|-|-|-
|about|For internal use only.|Object
|<a name="datelastcrawled"></a>dateLastCrawled|The last time that Bing crawled the webpage. The date is in the form, YYYY-MM-DDTHH:MM:SS. For example, 2015-04-13T05:23:39.|String
|<a name="deeplinks"></a>deepLinks|A list of links to related content that Bing found in the website that contains this webpage.<br/><br/>The `Webpage` object in this context includes only the `name`, `url`, `urlPingSuffix`, and `snippet` fields.|[Webpage](#webpage)[]
|<a name="displayurl"></a>displayUrl|The display URL of the webpage. The URL is meant for display purposes only and is not well formed.|String
|id|An ID that uniquely identifies this webpage in the list of web results.<br/><br/>The object includes this field only if the Ranking answer specifies that you mix the webpages with the other search results. Each webpage contains an ID that matches an ID in the Ranking answer. For more information, see [Ranking results](../rank-results.md).|String
|<a name="name"></a>name|The name of the webpage.<br/><br/>Use this name along with `url` to create a hyperlink that when clicked takes the user to the webpage.|String  
|mentions|For internal use only.|Object
|<a name="searchtags"></a>searchTags|A list of search tags that the webpage owner specified on the webpage. The API returns only indexed search tags.<br/><br/>The `name` field of the `MetaTag` object contains the indexed search tag. Search tags begin with search.* (for example, search.assetId). The `content` field contains the tag's value.|[MetaTag](#metatag)[]
|snippet|A snippet of text from the webpage that describes its contents.|String
|<a name="url"></a>url|The URL to the webpage.<br/><br/>Use this URL along with `name` to create a hyperlink that when clicked takes the user to the webpage.|String 

