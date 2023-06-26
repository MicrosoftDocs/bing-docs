---
title: Bing Entity Search API v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Entity Search API may return in the JSON response.
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-entity-search
ms.topic: reference
ms.date: 06/26/2023
---

# Entity Search API v7 response objects

For a list of possible objects, see **In this article** in the right pane.

If the request succeeds, the top-level object in the response is the [SearchResponse](#searchresponse) object. And if the request fails, the top-level object in the response is the [ErrorResponse](#errorresponse) object.

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.

## Entity  

Defines an entity such as a person, place, or thing.  
  
|Name|Value|Type
|-|-|-
|bingId|An ID that uniquely identifies this entity.|String  
|contractualRules|A list of rules that you must adhere to if you display the entity. For example, the rules may govern attribution of the entity's description.<br/><br/>The following contractual rules may apply:<ul><li>[LicenseAttribution](#licenseattribution)</li><li>[LinkAttribution](#linkattribution)</li><li>[MediaAttribution](#mediaattribution)</li><li>[TextAttribution](#textattribution)</li></ul>Not all entities include rules. If the entity provides contractual rules, you must abide by them. For more information about using contractual rules, see [Attributing Data](/bing/search-apis/bing-web-search/data-attribution).|Object[]
|description|A short description of the entity.|String  
|entityPresentationInfo|Additional information about the entity such as hints that you can use to determine the entity's type. To determine the entity's type, use the `entityScenario` and `entityTypeHint` fields. For example, the fields help you determine whether the entity is a dominant or disambiguation entity and whether it's a person or movie.<br/><br/>The entity is a dominant entity if Bing believes that only one entity satisfies the request. If multiple entities could satisfy the request, the entity is a disambiguation entity and the user needs to select the entity they're interested in.|[EntityPresentationInfo](#entitypresentationinfo)  
|image|An image of the entity.|[Image](#image)
|name|The entity's name.|String
|webSearchUrl|The URL that takes the user to the Bing search results page for this entity.|String

## EntityAnswer

Defines an entity answer that contains the list of entities.

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

## Identifiable  

Defines the identity of a resource.  
  
|Name|Value|Type
|-|-|-  
|id|An identifier.|String  

## Image  

Defines an image.  
  
> [!NOTE]
> Because the URL format and parameters are subject to change without notice, all image URLs should be used as-is; you should not take dependencies on the URL format or parameters. The exception is those parameters and values discussed by [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).  
  
|Name|Value|Type
|-|-|-
|height|The height of the source image, in pixels.|Unsigned Short
|hostPageUrl|The URL of the webpage that includes the image.<br/><br/>This URL and `contentUrl` may be the same URL.|String
|name|An optional text string that contains random information about the image.|String
|provider|The source of the image. The array will contain a single item.<br/><br/>You must attribute the image to the provider. For example, you may display the provider's name as the cursor hovers over the image or make the image a click-through link to the provider's website where the image is found.|[Organization](#organization)[]
|thumbnailUrl|The URL to a thumbnail of the image.|String
|width|The width of the source image, in pixels.|Unsigned Short

## License  

Defines the license under which the text or photo may be used.  
  
|Name|Value|Type
|-|-|-
|name|The name of the license.|String
|url|A URL to a website where the user can get more information about the license.<br/><br/>Use the name and URL to create a hyperlink.|String
  
## LicenseAttribution  

Defines a contractual rule for license attribution.  
  
|Name|Value|Type  
|-|-|-
|_type|A type hint, which is set to LicenseAttribution.|String
|license|The license under which the content may be used.|[License](#license)
|licenseNotice|The license to display next to the targeted field. For example, "Text under CC-BY-SA license".<br/><br/>Use the license's name and URL in the `license` field to create a hyperlink to the website that describes the details of the license. Then, replace the license name in the `licenseNotice` string (for example, CC-BY-SA) with the hyperlink you just created.|String
|mustBeCloseToContent|A Boolean value that determines whether the contents of the rule must be placed in close proximity to the field that the rule applies to. If **true**, the contents must be placed in close proximity. If **false**, or this field does not exist, the contents may be placed at the caller's discretion.|Boolean
|targetPropertyName|The name of the field that the rule applies to.|String
  
## Link  

Defines the components of a hyperlink.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint.|String
|text|The display text.|String
|url|A URL. Use the URL and display text to create a hyperlink.|String

## LinkAttribution  

Defines a contractual rule for link attribution.  
  
|Name|Value|Type
|-|-|-
|_type|A type hint, which is set to LinkAttribution.|String
|mustBeCloseToContent|A Boolean value that determines whether the contents of the rule must be placed in close proximity to the field that the rule applies to. If **true**, the contents must be placed in close proximity. If **false**, or this field does not exist, the contents may be placed at the caller's discretion.|Boolean
|targetPropertyName|The name of the field that the rule applies to.<br/><br/>If a target is not specified, the attribution applies to the entity as a whole and should be displayed immediately following the entity presentation. If there are multiple text and link attribution rules that do not specify a target, you should concatenate them and display them using a "Data from: " label. For example, “Data from <provider name1\> &#124; <provider name2\>".|String
|text|The attribution text.|String
|url|The URL to the provider's website. Use `text` and URL to create of hyperlink.|String

## LocalEntityAnswer

Defines an local entity answer that contains the list of places.

|Name|Value|Type
|-|-|-  
|value|A list of places.|[Place](#place)[]

## MediaAttribution  

Defines a contractual rule for media attribution.  
  
|Name|Value|Type
|-|-|-  
|_type|A type hint, which is set to MediaAttribution.|String
|mustBeCloseToContent|A Boolean value that determines whether the contents of the rule must be placed in close proximity to the field that the rule applies to. If **true**, the contents must be placed in close proximity. If **false**, or this field does not exist, the contents may be placed at the caller's discretion.|Boolean
|targetPropertyName|The name of the field that the rule applies to.|String
|url|The URL that you use to create of hyperlink of the media content. For example, if the target is an image, you would use the URL to make the image clickable.|String  

## Organization  

Defines a publisher.  
  
Note that a publisher may provide their name or their website or both.  
  
|Name|Value|Type
|-|-|-
|name|The publisher's name.|String
|url|The URL to the publisher's website.<br/><br/>Note that the publisher may not provide a website.|String

## Place  

Defines information about a local entity, such as a restaurant or hotel.

> [!NOTE]
> Entity responses support multiple markets, but the Places response supports only US Business locations.
  
|Name|Value|Type
|-|-|-
|_type|Type hint, which may be set to one of the following:<ul><li>Hotel</li><li>LocalBusiness</li><li>Restaurant</li></ul>|String
|address|The postal address of where the entity is located.|[PostalAddress](#postaladdress)
|entityPresentationInfo|Additional information about the entity such as hints that you can use to determine the entity's type. For example, whether it's a restaurant or hotel. The `entityScenario` field is set to ListItem.|[EntityPresentationInfo](#entitypresentationinfo)
|name|The entity's name.|String
|telephone|The entity's telephone number.|String
|url|The URL to the entity's website.<br/><br/>Use this URL along with the entity's name to create a hyperlink that when clicked takes the user to the entity's website.|String
|webSearchUrl|The URL to Bing's search result for this place.|String
  
## PostalAddress  

Defines a postal address.  
  
|Name|Value|Type
|-|-|-
|addressCountry|The country/region where the street address is located. This could be the two-letter ISO code (for example, US) or the full name (for example, United States).|String
|addressLocality|The city where the street address is located. For example, Seattle.|String
|addressRegion|The state or province code where the street address is located. This could be the two-letter code (for example, WA) or the full name (for example, Washington).|String
|neighborhood|The neighborhood where the street address is located. For example, Westlake.|String
|postalCode|The zip code or postal code where the street address is located. For example, 98052.|String
  
## QueryContext  

Defines the query string that Bing used for the request.
  
|Name|Value|Type
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

Defines a search result item to display. For more information about how to use the IDs, see [Ranking results](../how-to/rank-results.md).  
  
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
  
## SearchResponse  

The response's top-level object for search requests that succeed.  
  
If the service suspects a denial of service attack, the request succeeds (HTTP status code is 200 OK), but the body of the response is empty.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint, which is set to SearchResponse.|String
|<a name="searchresponse-entities"></a>entities|A list of entities that are relevant to the search query.|[EntityAnswer](#entityanswer)
|<a name="searchresponse-place"></a>places|A list of places that are relevant to the search query.|[LocalEntityAnswer](#localentityanswer)[]
|<a name="searchresponse-querycontext"></a>queryContext|The query string that Bing used for the request.|[QueryContext](#querycontext)
|<a name="searchresponse-ranking"></a>rankingResponse|The order that Bing suggests that you display the search results in.|[RankingResponse](#rankingresponse)

## TextAttribution  

Defines a contractual rule for plain text attribution.  
  
|Name|Value|Type
|-|-|-
|_type|A type hint, which is set to TextAttribution.|String
|text|The attribution text.<br/><br/>Text attribution applies to the entity as a whole and should be displayed immediately following the entity presentation. If there are multiple text or link attribution rules that do not specify a target, you should concatenate them and display them using a "Data from: " label.|String
