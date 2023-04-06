---
title: Bing Image Search APIs v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Image Search APIs may return in the JSON response.
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: reference
author: alekhyasasi
ms.date: 03/07/2023
---

# Image Search APIs v7 response objects

For a list of possible objects, see **In this article** in the right pane.

The top-level object in the response depends on the endpoint you call. If you call `/images/search`, the top-level object in the response is the [ImageAnswer](#imageanswer) object; and for `/images/trending`, it's [TrendingImages](#trendingimages). If the request fails, the top-level object is the [ErrorResponse](#errorresponse) object.

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.

## AggregateOffer  

Defines a list of offers from merchants that are related to the image.  
  
|Element|Description|Type
|-|-|-
|<a name="offers"></a>offers|A list of offers from merchants that have offerings related to the image.|[Offer](#offer)[]
  
## AggregateRating  

Defines the metrics that indicate how well an item was rated by others.  
  
|Name|Value|Type
|-|-|-
|<a name="bestrating"></a>bestRating|The highest rated review. The possible values are 1.0 through 5.0.|float
|<a name="ratingvalue"></a>ratingValue|The mean (average) rating. The possible values are 1.0 through 5.0.|float
|<a name="reviewcount"></a>reviewCount|The number of times the recipe has been rated or reviewed.|Unsigned Integer
|<a name="rating-text"></a>text|The mean (average) rating, in string form.|String

## Category  

Defines the category of trending images.  
  
|Name|Value|Type
|-|-|-
|<a name="tiles"></a>tiles|A list of images that are trending in the category. Each tile contains an image and a URL that returns more images of the subject. For example, if the category is Popular People Searches, the image is of a popular person and the URL would return more images of that person.|[Tile](#tile)[]
|<a name="category-title"></a>title|The name of the image category. For example, Popular People Searches.|String

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
|<a name="image-creativecommons"></a>creativeCommons|The <a href="https://creativecommons.org/licenses/" target="_blank">Creative Commons license</a> under which you may use the image. Please read and apply the license accordingly. The following are the possible license values.<ul><li><a href="https://creativecommons.org/licenses/by/2.0/" target="_blank">Attribution</a></li><li><a href="https://creativecommons.org/licenses/by-sa/3.0/" target="_blank">AttributionShareAlike</a></li><li><a href="https://creativecommons.org/licenses/by-nd/4.0/" target="_blank">AttributionNoDerivs</a></li><li><a href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">AttributionNonCommercial</a></li><li><a href="https://creativecommons.org/licenses/by-nc-sa/4.0/" target="_blank">AttributionNonCommercialShareAlike</a></li><li><a href="https://creativecommons.org/licenses/by-nc-nd/4.0/" target="_blank">AttributionNonCommercialNoDerivs</a></li><li><a href="https://creativecommons.org/share-your-work/public-domain/cc0/" target="_blank">PublicNoRightsReserved</a></li></ul>To filter images by license type, see the [license](query-parameters.md#license) query parameter.|String
|<a name="image-datepublished"></a>datePublished|The date and time, in UTC, that Bing discovered the image. The date is in the format, YYYY-MM-DDTHH:MM:SS.|String  
|<a name="image-encodingformat"></a>encodingFormat|The image's mime type (for example, jpeg).|String
|<a name="image-height"></a>height|The height of the source image, in pixels.|Unsigned Short
|<a name="image-hostpagedisplayurl"></a>hostPageDisplayUrl|The display URL of the webpage that hosts the image.<br/><br/>Use this URL in your user interface to identify the host webpage that contains the image. The URL is not a well-formed and should not be used to access the host webpage. To access the host webpage, use the `hostPageUrl` URL.|String
|<a name="image-hostpagedomainfriendlyname"></a>hostPageDomainFriendlyName|The friendly name of the domain that hosts the image. You should display the name over the image. Use the URL in `hostPageUrl` to make the name clickable. For example, Bing.com displays the clickable host name over the image when the user hovers over the image with the mouse.|String
|<a name="image-hostpageurl"></a>hostPageUrl|The URL of the webpage that includes the image. This URL and `contentUrl` may be the same URL.|String
|<a name="image-id"></a>id|An ID that uniquely identifies this image in the list of images.<br/><br/>The object includes this field only in a Web Search API response. For information about how to use this field, see [Ranking results](../../bing-web-search/rank-results.md) in the Web Search API guide.|String
|<a name="image-imageid"></a>imageId|An ID that uniquely identifies this image. If you want the image to be the first image in the response, set the [id](query-parameters.md#id) query parameter to this ID in your request.|String
|<a name="image-imageinsightstoken"></a>imageInsightsToken|The token that you use when calling the [Visual Search API](../../bing-visual-search/overview.md) to get insights about the image.|String
|<a name="image-insightsmetadata"></a>insightsMetadata|A count of the number of websites where you can shop or perform other actions related to the image.<br/><br/>For example, if the image is of an apple pie, this object includes a count of the number of websites where you can buy an apple pie. To indicate the number of offers in your UX, include badging such as a shopping cart icon that contains the count. When the user clicks on the icon, use the `imageInisghtsToken` in a [Visual Search API](../../bing-visual-search/overview.md) request to get the list of websites.|[InsightsMetadata](#insightsmetadata)
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
|currentOffset|The offset that represents where the first image in `value` is relative to all images that Bing might return for this query. Also see `nextOffset`.
|id|An ID that uniquely identifies the image answer. Only Web Search API responses include this field. For information about how to use this field, see [Ranking results](../../bing-web-search/rank-results.md) in the Web Search API guide.|String
|<a name="isfamilyfriendly"></a>isFamilyFriendly|A Boolean value that determines whether one or more of the images contain adult content. If one or more of the images contain adult content, `isFamilyFriendly` is set to **false**; otherwise, **true**.<br/><br/>If **false**, the thumbnail images are pixelated (fuzzy).<br/><br/>**NOTE:** This field is included only in a Web Search API response.|Boolean
|<a name="nextoffset"></a>nextOffset|The offset that you set the [offset](query-parameters.md#offset) query parameter to.<br/><br/>If you set `offset` to 0 and `count` to 30 in your first request, and then set `offset` to 30 in your second request, some of the results in the second response may be duplicates of the first response. To prevent duplicates, set `offset` to the value of `nextOffset`.<br/><br/>Only the Image Search API includes this field.|Integer
|<a name="pivotsuggestions"></a>pivotSuggestions|A list of segments in the original query. For example, if the query was *Red Flowers*, Bing might segment the query into *Red* and *Flowers*.<br/><br/>The Flowers pivot may contain query suggestions such as Red Peonies and Red Daisies, and the Red pivot may contain query suggestions such as Green Flowers and Yellow Flowers.<br/><br/>Only the Image Search API includes this field.|[Pivot](#pivot)  
|<a name="queryexpansions"></a>queryExpansions|A list of expanded queries that narrows the original query. For example, if the query was *Microsoft Surface*, the expanded queries might be: Microsoft Surface **Pro 3**, Microsoft Surface **RT**, Microsoft Surface **Phone**, and Microsoft Surface **Hub**.<br/><br/>Only the Image Search API includes this field.|[Query](#query)  
|<a name="querycontext"></a>queryContext|The query string that Bing used for the request.|[QueryContext](#querycontext)
|readLink|A partial URL that you'd use to request images from Image Search API using the search query string used for this response. The URL is not well formed; you need to prefix the URL with `https://api.bing.microsoft.com/v7.0` to create a well-formed URL. Include other query parameters as needed.|String
|relatedSearches|A list of related search queries made by others.|[Query](#query)[]
|<a name="similarterms">similarTerms|A list of terms that are similar in meaning to the user's query term.|[Query](#query)
|<a name="totalestimatedmatches"></a>totalEstimatedMatches|The estimated number of images that are relevant to the query. Use this number along with the [count](query-parameters.md#count) and [offset](query-parameters.md#offset) query parameters to page the results.<br/><br/>Only the Image Search API includes this field.|Long
|<a name="images-value">value|A list of images that are relevant to the query. If there are no results, the array is empty.|[Image](#image)[]
|webSearchUrl|A URL to the Bing search results for the requested images.|String

## ImageCaption  

Defines an image's caption.  
  
|Name|Value|Type
|-|-|-
|<a name="caption"></a>caption|A caption about the image.|String
|<a name="caption-datasourceurl"></a>dataSourceUrl|The URL to the website where the caption was found. You must attribute the caption to the source. For example, by displaying the domain name from the URL next to the caption and using the URL to link to the source website.|String
|<a name="caption-relatedsearches"></a>relatedSearches|A list of entities found in the caption. Use the contents of the `Query` object to find the entity in the caption and create a link. The link takes the user to images of the entity.|[Query](#query)

## ImageGallery  

Defines a link to a webpage that contains a collection of related images.  
  
|Name|Value|Type
|-|-|-
|<a name="gallery-creator"></a>creator|The person that owns the collection. You must attribute the collection to the creator.|[Person](#person)
|<a name="gallery-description"></a>description|A description of the collection. The description may be empty.|String
|<a name="gallery-followerscount"></a>followersCount|The number of users on the social network that follow the creator.|Unsigned Integer
|<a name="gallery-imagescount"></a>imagesCount|The number of related images found in the collection.|Unsigned Integer  
|<a name="gallery-name"></a>name|The name of the gallery.|String
|<a name="gallery-source"></a>source|The publisher or social network where the images were found. You must attribute the publisher as the source where the collection was found.|String
|<a name="gallery-thumbnailurl"></a>thumbnailUrl|The URL to a thumbnail of one of the images found in the collection.|String
|<a name="gallery-url"></a>url|The URL to the webpage that contains the collection of related images.|String

## ImagesModule  

Defines a list of images.  
  
|Element|Description|Type
|-|-|-  
|value|A list of images.|[Image](#image)[]
  
## ImageTagsModule  

Defines the characteristics of content found in an image.  
  
|Element|Description|Type
|-|-|-
|value|A list of tags that describe the characteristics of the content found in the image. For example, if the image is of a musical artist, the list might include Female, Dress, and Music to indicate the person is female music artist that's wearing a dress.|[Tag](#tag)[]  

## InsightsMetadata  

Defines a count of the number of websites where you can shop or perform other actions related to the image.  
  
|Name|Value|Type
|-|-|-
|<a name="insightsmetadata-aggregateoffer"></a>aggregateOffer|A summary of the online offers of products found in the image. For example, if the image is of a dress, the offer might identify the lowest price and the number of offers found.<br /><br /> Only visually similar products insights include this field.<br /><br /> The offer includes the following fields: `Name`, `AggregateRating`, `OfferCount`, and `LowPrice`.|[Offer](#offer)  
|<a name="availablesizescount"></a>availableSizesCount|The number of different sizes of the image that Bing found on one or more websites.|Unsigned Integer  
|<a name="pagesincludingcount"></a>pagesIncludingCount|The number of webpages that include the image.|Unsigned Integer  
|<a name="recipesourcecount"></a>recipeSourcesCount|The number of websites that offer recipes of the food seen in the image.|Unsigned Integer  
|<a name="shoppingsourcecount"></a>shoppingSourcesCount|The number of websites that offer the products seen in the image.|Unsigned Integer

## MediaSize  

Defines the size of the media content.  
  
|Name|Value|Type
|-|-|-
|height|The height of the media content, in pixels.|Integer
|width|The width of the media content, in pixels.|Integer

## NormalizedRectangle  

Defines a region of an image. The region is defined by the coordinates of the top, left corner and bottom, right corner of the region. The coordinates are fractional values of the original image's width and height in the range 0.0 through 1.0.  
  
|Name|Value|Type
|-|-|-
|bottom|The bottom coordinate.|Float
|left|The left coordinate.|Float
|right|The right coordinate|Float
|top|The top coordinate|Float

## Offer  

Defines a merchant's offer.  
  
The ShoppingSources insights and SimilarProducts insights both use this object. To determine the insight that the field applies to, see the field's description.  
  
|Element|Description|Type
|-|-|-
|aggregateRating|An aggregated rating that indicates how well the product has been rated by others.<br /><br /> Used by SimilarProducts.|[AggregateRating](#aggregaterating)
|availability|The item's availability. The following are the possible values.<br /><ul><li>Discontinued</li><li>InStock</li><li>InStoreOnly</li><li>LimitedAvailability</li><li>OnlineOnly</li><li>OutOfStock</li><li>PreOrder</li><li>SoldOut</li></ul><br /> Used by ShoppingSources.|String
|description|A description of the item.|String  
|lastUpdated|The last date that the offer was updated. The date is in the form YYYY-MM-DD.|Sting
|lowPrice|The lowest price of the item that Bing found online.<br /><br /> Used by SimilarProducts.|Float
|name|The name of the product.|String
|offerCount|The number of offers that Bing found online.<br /><br /> Used by SimilarProducts.|Unsigned Integer
|price|The item's price.<br /><br /> Used by ShoppingSources.|Float
|priceCurrency|The monetary currency. For example, USD.|String
|seller|The merchant's name.<br /><br /> Used by ShoppingSources.|[Organization](#organization)
|url|The URL to the offer on the merchant's website.<br /><br /> Used by ShoppingSources.|String

## Organization  

Defines information about a merchant.  
  
|Element|Description|Type
|-|-|-
|image|The merchant's logo. The `Image` object includes only the `url` field.|[Image](#image)
|name|The merchant's name.|String

## Person  

Defines a person.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint.|String
|description|A short description of the person.|String
|image|An image of the person.|[Image](#image)
|jobTitle|The person's job title.|String
|name|The person's name.|String
|twitterProfile|The URL of the person's twitter profile.|String
|url|The URL to the person's social network home page, if applicable.|String
|webSearchUrl|The URL to the Bing search results page that contains information about this person.|String

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
|<a name="query-displaytext"></a>displayText|The display version of the search query string.|String
|<a name="query-searchurl"></a>searchLink|The Image Search API URL that you use to get image search results. The URL includes the *q* query parameter that's set the search string in `text`. Before using the URL, append other query parameters as appropriate.<br/><br/>Use this URL if you're displaying the results in your own user interface. Otherwise, use the URL in `webSearchUrl`.|String
|<a name="query-text"></a>text|The query string. Use this string as the query term in a new search request.|String  
|<a name="query-thumbnail"></a>thumbnail|A URL to a thumbnail image that represents the search string.|[Thumbnail](#thumbnail)
|<a name="query-websearchurl"></a>webSearchUrl|The URL that takes the user to the Bing search results page for the query.|String

## QueryContext  

Defines the query string that Bing used for the request.
  
|Name|Value|Type
|-|-|-
|<a name="adultintent"></a>adultIntent|A Boolean value that indicates whether the specified query has adult intent. The value is **true** if the query has adult intent.<br/><br/>If **true**, and the request's [safeSearch](query-parameters.md#safesearch) query parameter is set to Strict, the response contains only news results, if applicable.|Boolean
|<a name="alterationoverridequery"></a>alterationOverrideQuery|The query string to use to force Bing to use the original string. For example, if the query string is *sailing downwind*, the override query string is *+sailing downwind*. Remember to encode the query string, which results in *%2Bsailing+downwind*.<br/><br/>The object includes this field only if the original query string contains a spelling mistake.|String
|<a name="alteredquery"></a>alteredQuery|The query string that Bing used to perform the query. Bing uses the altered query string if the original query string contained spelling mistakes. For example, if the query string is *sailing downwind*, the altered query string is *sailing downwind*.<br/><br/>The object includes this field only if the original query string contains a spelling mistake.|String
|askUserForLocation|A Boolean value that indicates whether Bing requires the user's location to provide accurate results. If you specified the user's location by using the [X-MSEdge-ClientIP](headers.md#clientip) and [X-Search-Location](headers.md#location) headers, you can ignore this field.<br/><br/>For location aware queries, such as "today's weather" or "restaurants near me" that need the user's location to provide accurate results, this field is set to **true**.<br/><br/>For location aware queries that include the location (for example, "Seattle weather"), this field is set to **false**. This field is also set to **false** for queries that are not location aware, such as "best sellers."|Boolean
|<a name="originalquery"></a>originalQuery|The query string as specified in the request.|String  

## Recipe  

Defines a cooking recipe.  
  
|Element|Description|Type
|-|-|-
|aggregateRating|Aggregated ratings that indicate how well the recipe has been rated by others.|[AggregateRating](#aggregaterating)
|cookTime|The amount of time the food takes to cook. For example, PT25M. For information about the time format, see [https://en.wikipedia.org/wiki/ISO_8601#Durations](https://en.wikipedia.org/wiki/ISO_8601#Durations).|String
|creator|The recipe's author.|[Person](#person)
|name|The name of the recipe.|String
|prepTime|The amount of time required to prepare the ingredients. For example, PT15M. For information about the time format, see [https://en.wikipedia.org/wiki/ISO_8601#Durations](https://en.wikipedia.org/wiki/ISO_8601#Durations).|String
|thumbnailUrl|The URL to a thumbnail image of the prepared food.|String
|totalTime|The total amount of time it takes to prepare and cook the recipe. For example, PT45M. For information about the time format, see [https://en.wikipedia.org/wiki/ISO_8601#Durations](https://en.wikipedia.org/wiki/ISO_8601#Durations).|String
|url|The URL that takes the user to the webpage that contains the recipe.|String

## RecipesModule  

Defines a list of recipes.  
  
|Element|Description|Type
|-|-|-
|value|A list of recipes.|[Recipe](#recipe)[]

## RecognizedEntitiesModule  

Defines a list of previously recognized entities.  
  
|Element|Description|Type
|-|-|-
|value|A list of recognized entities.|[RecognizedEnityGroup](#recognizedentitygroup)[]

## RecognizedEntity  

Defines a recognized entity.  
  
|Element|Description|Type
|-|-|-
|entity|The entity that was recognized.<br/><br/>The following are the possible entity objects.<ul><li>[Person](#person)</li></ul>|Object
|matchConfidence|The confidence that Bing has that the entity in the image matches this entity. The confidence ranges from 0.0 through 1.0 with 1.0 being very confident.|Float  

## RecognizedEntityGroup  

Defines a group of previously recognized entities.  
  
|Element|Description|Type
|-|-|-
|name|The name of the group where images of the entity were also found.<br/><br/>The following are possible groups.<ul><li>CelebRecognitionAnnotations &mdash; Similar to CelebrityAnnotations but provides a higher probability of an accurate match.</li><li>CelebrityAnnotations &mdash; Contains celebrities such as actors, politicians, athletes, and historical figures.</li></ul>|String
|recognizedEntityRegions|The regions of the image that contain entities.|[RecognizedEntityRegion](#recognizedentityregion)[]
  
## RecognizedEntityRegion  

Defines a region of the image where an entity was found and a list of entities that might match it.  
  
|Element|Description|Type
|-|-|-
|matchingEntities|A list of entities that Bing believes match the entity found in the region. The entities are in descending order of confidence (see the `matchConfidence` field of RecognizedEntity).|[RecognizedEntity](#recognizedentity)[]
|region|A region of the image that contains an entity.<br/><br/> The values of the rectangle are relative to the width and height of the original image and are in the range 0.0 through 1.0.<br/><br/> For example, if the image is 300x200 and the region's top, left corner is at point (10, 20) and the bottom, right corner is at point (290, 150), then the normalized rectangle is:<br/><br/>Left = 0.0333333333333333<br/>Top = 0.1<br/>Right = 0.9666666666666667<br/>Bottom = 0.75<br/><br/>For people, the region represents the person's face.|[NormalizedRectangle](#normalizedrectangle)  

## RelatedCollectionsModule  

Defines a list of webpages that contain related images.  
  
|Element|Description|Type
|-|-|-
|value|A list of webpages that contain related images.|[ImageGallery](#imagegallery)[]

## RelatedSearchesModule  

Defines a list of related searches made by others.  
  
|Element|Description|Type
|-|-|-
|value|A list of related searches made by others.|[Query](#query)[]

## Tag  

Defines a characteristic of the content found in the image.  
  
|Element|Description|Type
|-|-|-
|name|The name of the characteristic. For example, cat, kitty, calico cat.|String
  
## Thumbnail  

Defines the URL to a thumbnail of an image.  
  
|Name|Value|Type
|-|-|-
|url|The URL to a thumbnail of an image.|String
  
## Tile  

Defines a video tile.  
  
|Name|Value|Type
|-|-|-
|<a name="tile-image"></a>image|A URL to the image's thumbnail.|[Image](#image)  
|<a name="tile-query"></a>query|A query that returns a Bing search results page with images of the subject. For example, the thumbnail is of a popular person if the category is Popular People Searches. The query would return a Bing search results page with more images of that person.|[Query](#query)  
  
## TrendingImages  

The top-level object that the response includes when a trending images request succeeds.  
  
|Name|Value|Type
|-|-|-
|categories|A list that identifies categories of images and a list of trending images in those categories.|[Category](#category)[]|  
