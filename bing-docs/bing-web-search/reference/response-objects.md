---
title: Bing Web Search API v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Web Search API may return in the JSON response.
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: reference
author: alekhyasasi
ms.date: 04/24/2024
ms.author: v-alpunnamar
---

# Web Search API v7 response objects

For a list of possible objects, see **In this article** in the right pane.

If the request succeeds, the top-level object in the response is the [SearchResponse](#searchresponse) object. And if the request fails, the top-level object in the response is the [ErrorResponse](#errorresponse) object.

The JSON objects in this section are specific to the web answer. For details about the JSON objects for other answer types that the search results may include, see the API-specific reference documentation. For example, if the search results contain the images and news answers, see the [Image Search API reference](../../bing-image-search/reference/endpoints.md) and [News Search API reference](../../bing-news-search/reference/endpoints.md).

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.

## Attribution

Defines the publisher that the content is attributed to.
  
|Name|Value|Type
|-|-|-
|providerDisplayName|The publisher's name that you use to attribute the content to.|String
|seeMoreUrl|The URL to the publisher's website. Use `providerDisplayName` and this URL to create a hyperlink that you display in the UX following the translation.|String

## Computation  

Defines an expression and its answer.  
  
|Name|Value|Type
|-|-|-
|<a name="computation-expression"></a>expression|The math or conversion expression.<br/><br/>If the query contains a request to convert units of measure (for example, meters to feet), this field contains the *from* units and `value` contains the *to* units.<br/><br/>If the query contains a mathematical expression such as 2+2, this field contains the expression and `value` contains the answer.<br/><br/>Note that mathematical expressions may be normalized. For example, if the query was sqrt(4^2+8^2), the normalized expression may be sqrt((4^2)+(8^2)).<br/><br/>If the user's query is a math question and the [textDecorations](query-parameters.md#textdecorations) query parameter is set to **true**, the expression string may include formatting markers. For example, if the user's query is *log(2)*, the normalized expression includes the subscript markers. For more information, see [Hit highlighting](../hit-highlighting.md).|String
|<a name="computation-value"></a>value|The expression's answer.|String  

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
> Because URL formats and parameters are subject to change without notice, all image URLs should be used as-is; you should not take dependencies on the URL format or parameters. The exception is those parameters and values discussed by [Resize and crop thumbnail images](../resize-and-crop-thumbnails.md).  
  
|Name|Value|Type
|-|-|-
|height|The height of the source image, in pixels.|Unsigned Short
|hostPageUrl|The URL of the webpage that includes the image.<br/><br/>This URL and `contentUrl` may be the same URL.|String
|name|An optional text string that contains random information about the image.|String
|provider|The source of the image. The array will contain a single item.<br/><br/> You must attribute the image to the provider. For example, you may display the provider's name as the cursor hovers over the image or make the image a click-through link to the provider's website where the image is found.<br/><br/>If the image object is part of a larger object, and the larger object contains a contractual rule that applies to this object, you must use the contractual rule for attribution instead of this field.|[Organization](#organization)[]  
|thumbnailUrl|The URL to a thumbnail of the image. For information about resizing the image, see [Resize and crop thumbnail images](../resize-and-crop-thumbnails.md).|String
|width|The width of the source image, in pixels.|Unsigned Short

## License

Defines the license under which you may use the content.  
  
|Name|Value|Type  
|-|-|-
|name|The name of the license.|String
|url|A URL to the website that describes the license. Use `name` and `url` to create a hyperlink.|String

## LicenseAttribution  

Defines a contractual rule for license attribution.  
  
|Name|Value|Type  
|-|-|-
|_type|A type hint, which is set to LicenseAttribution.|String
|license|The license under which the content may be used.|[License](#license)
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

## Malware  

Defines a notice that the webpage may cause a potential issue if the user clicks the `url` link.  
  
|Name|Value|Type
|-|-|-
|beSafeRxUrl|A URL to a webpage where the user may get more information about safely buying prescription medicine online.|String
|malwareWarningType|The type of malware notice. Possible values are:<ul><li>NABP &mdash; Warns that the National Association of Boards of Pharmacy includes this pharmacy on its Not Recommended list.</li><li>Malware &mdash; Warns that the site may download malicious software that may harm the user’s device.</li><li>MaliciousPageLink &mdash; Warns that the site may contain links that could download malicious software that may harm the user’s device.</li><li>Phishing &mdash; Warns that the site could trick the user into disclosing financial, personal, or other sensitive information.</li></ul>|String
|warningExplanationUrl|A URL to a webpage where the user can get an explanation of the issue. For NABP notices, users can use this link to verify a pharmacy.|String
|warningLetterUrl|A URL to a webpage where the user can get more information about the notice. For NABP notices, users can use this link to see the list of online sites that the board doesn’t recommend.|String

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
|<a name="query-displaytext"></a>displayText|The display version of the query term. This version of the query term may contain special characters that highlight the search term found in the query string. The string contains the highlighting characters only if the query enabled hit highlighting (see the [textDecorations](query-parameters.md#textdecorations) query parameter). For details about hit highlighting, see [Hit highlighting](../hit-highlighting.md).|String
|<a name="query-text"></a>text|The query string. Use this string as the query term in a new search request.|String  
|<a name="query-websearchurl"></a>webSearchUrl|The URL that takes the user to the Bing search results page for the query.<br/><br/>Only related search results include this field.|String
  
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
|id|An ID that uniquely identifies the related search answer.<br/><br/>The object includes this field only if the Ranking answer specifies that you should display all related searches in a group. For more information about how to use the ID, see [Ranking results](../rank-results.md).|String
|<a name="relatedsearch-value"></a>value|A list of related queries that were made by others.|[Query](#query)[]  
  
## SearchResponse  

The response's top-level object for search requests that succeed.  
  
By default, the Search API includes all relevant answers unless:  
  
- The query specifies the [responseFilter](query-parameters.md#responsefilter) query parameter to limit the answers it returns.
- One or more of the search components does not return results (for example, no news results are relevant to the query).  
- The subscription key does not have access to the search component.  
  
If the service suspects a denial of service attack, the request succeeds (HTTP status code is 200 OK), but the body of the response is empty.  
  
|Name|Value|Type
|-|-|-
|\_type|Type hint, which is set to SearchResponse.|String
|<a name="searchresponse-computation"></a>computation|The answer to a math expression or unit conversion expression.|[Computation](#computation)
|<a name="searchresponse-entities"></a>entities|A list of entities that are relevant to the search query.|[EntityAnswer](../../bing-entity-search/reference/response-objects.md#entityanswer)
|<a name="searchresponse-images"></a>images|A list of images that are relevant to the search query.|[ImageAnswer](../../bing-image-search/reference/response-objects.md#imageanswer)
|<a name="searchresponse-news"></a>news|A list of news articles that are relevant to the search query.|[NewsAnswer](../../bing-news-search/reference/response-objects.md#newsanswer)
|<a name="searchresponse-places"></a>places|A list of places that are relevant to the search query.|[LocalEntityAnswer](../../bing-entity-search/reference/response-objects.md#localentityanswer)
|<a name="searchresponse-querycontext"></a>queryContext|The query string that Bing used for the request.|[QueryContext](#querycontext)
|<a name="searchresponse-ranking"></a>rankingResponse|The order that Bing suggests that you display the search results in.|[RankingResponse](#rankingresponse)
|<a name="searchresponse-relatedsearches"></a>relatedSearches|A list of related queries made by others.|[RelatedSearchAnswer](#relatedsearchanswer)
|<a name="searchresponse-spellsuggestions"></a>spellSuggestions|The query string that likely represents the user's intent.|[SpellSuggestions](#spellsuggestions)  
|<a name="searchresponse-timezone"></a>timeZone|The date and time of one or more geographic locations.|[TimeZone](#timezone)
|<a name="searchresponse-translations"></a>translations|The translation of a word or phrase in the query string to another language.|[TranslationAnswer](#translationanswer)
|<a name="searchresponse-videos"></a>videos|A list of videos that are relevant to the search query.|[VideosAnswer](../../bing-video-search/reference/response-objects.md#videosanswer)
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
|date|A date in string form. For example, Thursday, June 5, 2019. The answer may include this field if the user’s query asks Bing to compute a date. For example, *90 days from today*.|String
|description|A description of the response. The answer may include this field if the query asks Bing how many days or weeks in a period (for example, *weeks in a year* or *days in a month*), or converts time to a different time zone (for example, *pst to est*).|String
|<a name="othercitytimes"></a>otherCityTimes|A list of dates and times in a geographical location. The answer includes this field for queries like *US time zones* or *Arizona time zones*. The list is ordered by UTC offset.|[TimeZoneInformation](#timezoneinformation)[]
|<a name="primarycitytime"></a>primaryCityTime|The data and time, in UTC, of the geographic location specified in the query.<br/><br/>If the query specified a specific geographic location (for example, a city), this object contains the name of the geographic location and the current date and time of the location.<br/><br/>If the query specified a general geographic location, such as a state or country/region, this object contains the date and time of the primary city or state found within the specified state or country/region. If the location contains additional time zones, the `otherCityTimes` field contains the date and time of cities or states located in the other time zones.|[TimeZoneInformation](#timezoneinformation)
|primaryResponse|The primary data that satisfies the request. If the query string is *how many weeks in 2019*, this field contains, **52 weeks and 1 day**. Other query examples: *how many days in this month* and *what’s the date*.|String
|primaryTimeZone|The object contains the primary time zone in a geographical location. If a location contains more than one time zone, Bing determines which time zone is the primary time zone. The answer includes this field for queries like, time zone, time zones in Arizona, us time zones.|[TimeZoneInformation](#timezoneinformation)
|timeZoneDifference|The difference in time, in hours, between time zones. For example, there's a three hour difference between PST and EST.|[TimeZoneDifference](#timezonedifference)
  
## TimeZoneDifference  

Defines the difference in time, in hours, between time zone 1 and time zone 2.  
  
|Name|Value|Type
|-|-|-
|location1|the date and time of the first time zone. For example, if the query is, *pst to est*, this field contains the date and time of the Pacific time zone.|[TimeZoneInformation](#timezoneinformation)
|location2|the date and time of the second time zone. For example, if the query is, *pst to est*, this field contains the date and time of the Eastern time zone.|[TimeZoneInformation](#timezoneinformation)
|text|A string that represents the difference in time between time zones.|String  

## TimeZoneInformation  

Defines a date and time for a geographical location.  
  
|Name|Value|Type
|-|-|-
|<a name="tzinfo-location"></a>location|The type of the geographical location.<br/><br/>For example, County; City; City, State; City, State, Country/Region; or Time Zone.|String  
|<a name="tzinfo-time"></a>time|The UTC date and time specified in the form, YYYY-MM-DDThh;mm:ss.ssssssZ.|String
|timeZoneName|The name of the time zone that the location is in. This string may be empty if the query is not related to time zones.|String
|<a name="tzinfo-utcoffset"></a>utcOffset|The offset from UTC. For example, UTC-7.|String

## TranslationAnswer

Defines the translation of a word or phrase in the query string to another language.  
  
|Name|Value|Type
|-|-|-
|attributions|A list of publishers that you must attribute the information to when you render the answer.<br/><br/>You must display the names of all publishers in the list as the source of the data. Typically, you display the providers in a single line after the translation. For example, “Data from: \<provider\> \| \<provider\> \| …", where \<provider\> is the name of the provider in providerDisplayName.<br/><br/>**Note**: If the answer includes contractualRules, you must apply them instead of applying attributions from this field.|[Attribution](#attribution)[]
|contractualRules|A list of rules that you must adhere to if you display the answer. The following contractual rules may apply.<ul><li>[LinkAttribution](#linkattribution)</li><ul>For information about displaying contractual rules, see [Data Attribution](../data-attribution.md).|Object[]
|id|An ID that uniquely identifies this answer.<br/><br/>The [RankingResponse](#rankingresponse) answer uses the ID to indicate where in the rendered response you should display this answer. For information about how to use this field, see [How to use ranking to display search results](../rank-results.md).|String
|inLanguage|The language that the text was translated from. An ISO 639-1 two-letter language code identifies the language. For example, es for Spanish.|String
|originalText|The text to translate.|String
|translatedLanguageName|The language that the text was translated to. An ISO 639-1 two-letter language code identifies the language. For example, en for English.|String
|translatedText|The translated text.|String

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
|about|For internal use only.|Object[]
|<a name="datelastcrawled"></a>dateLastCrawled|The last time that Bing crawled the webpage. The date is in the form, YYYY-MM-DDTHH:MM:SS. For example, 2015-04-13T05:23:39.|String
|datePublished | The time that webpage published. The date is in the form, YYYY-MM-DDTHH:MM:SS. <br/>Example: 2015-04-13T05:23:39.| String
|datePublishedDisplayText | The display version of the datePublished.| String
|contractualRules|A list of rules that you must adhere to if you display the answer. The following contractual rules may apply.<ul><li>[LicenseAttribution](#licenseattribution)</li></ul><br>For information about displaying contractual rules, see [Data Attribution](../data-attribution.md).|Object[]
|<a name="deeplinks"></a>deepLinks|A list of links to related content that Bing found in the website that contains this webpage.<br/><br/>The `Webpage` object in this context includes only the `name` and `url` fields and optionally the `snippet` field.|[Webpage](#webpage)[]
|<a name="displayurl"></a>displayUrl|The display URL of the webpage. The URL is meant for display purposes only and is not well formed.|String
|id|An ID that uniquely identifies this webpage in the list of web results.<br/><br/>The object includes this field only if the Ranking answer specifies that you mix the webpages with the other search results. Each webpage contains an ID that matches an ID in the Ranking answer. For more information, see [Ranking results](../rank-results.md).|String
|isFamilyFriendly|A Boolean value that indicates whether the webpage contains adult content. If the webpage doesn't contain adult content, `isFamilyFriendly` is set to **true**.|Boolean
|isNavigational|A Boolean value that indicates whether the user’s query is frequently used for navigation to different parts of the webpage’s domain. Is **true** if users navigate from this page to other parts of the website.|Boolean
|language|A two-letter language code that identifies the language used by the webpage. For example, the language code is *en* for English.|String
|<a name="webpage_malware"></a>malware|A notice that Bing provides if it thinks the webpage may cause a potential issue if the user clicks the `url` link. You should display the notice with high visibility next to the webpage link.|[Malware](#malware)
|<a name="name"></a>name|The name of the webpage.<br/><br/>Use this name along with `url` to create a hyperlink that when clicked takes the user to the webpage.|String  
|mentions|For internal use only.|Object
|<a name="searchtags"></a>searchTags|A list of search tags that the webpage owner specified on the webpage. The API returns only indexed search tags.<br/><br/>The `name` field of the `MetaTag` object contains the indexed search tag. Search tags begin with search.* (for example, search.assetId). The `content` field contains the tag's value.|[MetaTag](#metatag)[]
|snippet|A snippet of text from the webpage that describes its contents.|String
|<a name="url"></a>url|The URL to the webpage.<br/><br/>Use this URL along with `name` to create a hyperlink that when clicked takes the user to the webpage.|String
