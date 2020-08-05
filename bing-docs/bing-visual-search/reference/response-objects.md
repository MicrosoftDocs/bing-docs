---
title: Bing Visual Search API v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Visual Search API may return in the JSON response.
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-visual-search
ms.topic: reference
ms.date: 07/15/2017
ms.author: scottwhi
---

# Visual Search API v7 response objects

For a list of possible objects, see **In this article** in the right pane.

If the request succeeds, the top-level object in the response is the [ImageKnowledge](#imageknowledge) object. And if the request fails, the top-level object in the response is the [ErrorResponse](#errorresponse) object.

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.


## AggregateOffer

Defines a list of offers from merchants that are related to the image.

|Name|Value|Type 
|-|-|-  
|aggregateRating|An aggregated rating that indicates how well the product has been rated by others.|[AggregateRating](#aggregaterating)
|availability|The item's availability. The following are the possible values:<ul><li>Discontinued</li><li>InStock</li><li>InStoreOnly</li><li>LimitedAvailability</li><li>OnlineOnly</li><li>OutOfStock</li><li>PreOrder</li><li>SoldOut</li></ul>|String
|lastUpdated|The last date that the offer was updated. The date is in the form YYYY-MM-DD.|String
|offers|A list of offers from merchants that have offerings related to the image.|[Offer](#offer)
|price|The item's price.|Double
|priceCurrency|The three-letter ISO 4217 currency code that `price` is specified in. For example, USD.|String
|seller|The seller for this offer.|[Organization](#organization)


## AggregateRating

Defines metrics that indicate how well an item was rated by others.

|Name|Value|Type 
|-|-|- 
|_type|A type hint, which is set to Properties/Item|String
|bestRating|The highest rated review. The possible values are 1.0 through 5.0.|Double 
|ratingValue|The mean (average) rating. The possible values are 1.0 through 5.0.|Double 
|reviewCount|The number of times the item was rated or reviewed.|Integer
|text|Text representation of an item.|String

 
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

Defines an image.  
  
> [!NOTE]
> Because URL formats and parameters are subject to change without notice, all image URLs should be used as-is; you should not take dependencies on the URL format or parameters. The exception is those parameters and values discussed by [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).  
  
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
|<a name="image-id"></a>id|An ID that uniquely identifies this image in the list of images.<br/><br/>The object includes this field only in a Web Search API response. For information about how to use this field, see [Ranking results](../../bing-web-search/rank-results.md) in the Web Search API guide.|String
|<a name="image-imageid"></a>imageId|An ID that uniquely identifies this image.|String
|<a name="image-imageinsightstoken"></a>imageInsightsToken|The token that you use when calling the Visual Search API to get insights about the image.|String
|<a name="image-insightsmetadata"></a>insightsMetadata|A count of the number of websites where you can shop or perform other actions related to the image.<br/><br/>For example, if the image is of an apple pie, this object includes a count of the number of websites where you can buy an apple pie. To indicate the number of offers in your UX, include badging such as a shopping cart icon that contains the count. When the user clicks on the icon, use the `imageInisghtsToken` in a Visual Search API request to get the list of websites.|[InsightsMetadata](#insightsmetadata) 
|<a name="image-name"></a>name|A title of the image.|String
|<a name="image-thumbnail"></a>thumbnail|The width and height of the thumbnail image (see `thumbnailUrl`).|[MediaSize](#mediasize) 
|<a name="image-thumbnailurl"></a>thumbnailUrl|A URL to a thumbnail of the image. For information about resizing the image, see [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).|String
|webSearchUrl|A URL to the Bing search results for this image.|String  
|<a name="image-width"></a>width|The width of the source image, in pixels.|Unsigned Short 
  

## ImageEntityAction  

Defines an entity action.  
  
|Name|Value|Type 
|-|-|-  
|_type|A type hint, which is set to ImageEntityAction.|String  
|actionType|A string representing the type of action.|String
|datePublished|The date on which the CreativeWork was published.|String
|displayName|A display name for the action.|String
|isTopAction|A Boolean representing whether this result is the top action.|Boolean
|provider|The source of the creative work.|[Thing](#thing)[]
|result|The result produced in the action.|[Thing](#thing)[]
|serviceUrl|Use this URL to get additional data to determine how to take the appropriate action. For example, the `serviceUrl` might return JSON along with an image URL.|String
|text|Text content of this creative work.|String
|thumbnailUrl|A URL to a thumbnail of the item.|String


## ImageKnowledge

Defines the top-level object in the API response.

|Name|Value|Type 
|-|-|-  
|_type|A type hint, which is set to ImageKnowledge|String
|id|A String identifier.|String
|image|The image used in this insights request.|[Image](#image)
|readLink|The URL that returns this resource. To use the URL, append query parameters as appropriate and include the Ocp-Apim-Subscription-Key header.|String
|tags|A list of visual search tags.|[ImageTag](#imagetag)[]
|webSearchUrl|A URL to Bing's search result for this item.|String


## ImageModuleAction

Defines an image list action.

|Name|Value|Type 
|-|-|-  
|_type|A type hint, which is set to ImageModuleAction.|String
|actionType|A string representing the type of action.|String
|data|A list of images.|[ImagesModule](#imagesmodule)
|datePublished|The date on which the CreativeWork was published.|String
|displayName|A display name for the action.|String
|isTopAction|A Boolean representing whether this result is the top action.|Boolean
|provider|The source of the creative work.|[Thing](#thing)[]
|result|The result produced in the action.|[Thing](#thing)[]
|serviceUrl|Use this URL to get additional data to determine how to take the appropriate action. For example, the `serviceUrl` might return JSON along with an image URL.|String
|text|Text content of this creative work.|String
|thumbnailUrl|A URL to a thumbnail of the item.|String


## ImageRecipesAction

Defines a recipe action.

|Name|Value|Type 
|-|-|-  
|_type|A type hint, which is set to ImageRecipeAction.|String
|actionType|A string representing the type of action.|String
|data|A list of recipes related to the image.|[RecipesModule](#recipesmodule)
|datePublished|The date on which the CreativeWork was published.|String
|displayName|A display name for the action.|String
|isTopAction|A Boolean representing whether this result is the top action.|Boolean
|provider|The source of the creative work.|[Thing](#thing)[]
|result|The result produced in the action.|[Thing](#thing)[]
|serviceUrl|Use this URL to get additional data to determine how to take the appropriate action. For example, the `serviceUrl` might return JSON along with an image URL.|String
|text|Text content of this creative work.|String
|thumbnailUrl|A URL to a thumbnail of the item.|String


## ImageRelatedSearchesAction

Defines a related search action.

|Name|Value|Type 
|-|-|-  
|_type|A type hint, which is set to ImageRelatedSearchesAction.|String
|actionType|A string representing the type of action.|String
|data|A list of searches that are related to the image.|[RelatedSearchesModule](#relatedsearchesmodule)
|datePublished|The date on which the CreativeWork was published.|String
|displayName|A display name for the action.|String
|isTopAction|A Boolean representing whether this result is the top action.|Boolean
|provider|The source of the creative work.|[Thing](#thing)[]
|result|The result produced in the action.|[Thing](#thing)[]
|serviceUrl|Use this URL to get additional data to determine how to take the appropriate action. For example, the `serviceUrl` might return JSON along with an image URL.|String
|text|Text content of this creative work.|String
|thumbnailUrl|A URL to a thumbnail of the item.|String


## ImageShoppingSourcesAction

Defines a shopping sources action.

|Name|Value|Type 
|-|-|-  
|_type|A type hint, which is set to ImageShoppingSourcesAction.|String
|actionType|A string representing the type of action.|String
|data|A list of merchants that offer items seen in the image.|[AggregateOffer](#aggregateoffer)
|datePublished|The date on which the CreativeWork was published.|String
|displayName|A display name for the action.|String
|isTopAction|A Boolean representing whether this result is the top action.|Boolean
|provider|The source of the creative work.|[Thing](#thing)[]
|result|The result produced in the action.|[Thing](#thing)[]
|serviceUrl|Use this URL to get additional data to determine how to take the appropriate action. For example, the `serviceUrl` might return JSON along with an image URL.|String
|text|Text content of this creative work.|String
|thumbnailUrl|A URL to a thumbnail of the item.|String


## ImagesModule

Defines a list of images.

|Name|Value|Type
|-|-|-
|value|List of images.|[Image](#image)[]


## ImageTag

Defines an image tag.

|Name|Value|Type
|-|-|-
|_type|A type hint.|String
|actions|Actions within this tag. The order of the items denotes the default ranking order of these actions, with the first action being the most likely user intent. The following are the possible actions:<ul><li>[ImageEntityAction](#imageentityaction)</li><li>[ImageModuleAction](#imagemoduleaction)</li><li>[ImageRecipesAction](#imagerecipesaction)</li><li>[ImageRelatedSearchesAction](#imagerelatedsearchesaction)</li><li>[ImageShoppingSourcesAction](#imageshoppingsourcesaction)</li></ul>|Object[]
|alternateName|An alias for the item.|String
|bingId|An ID that uniquely identifies this item.|String
|boundingBox|The bounding box for this tag. For the default tag, there is no bounding box.|[ImageTagRegion](#imagetagregion)
|description|A short description of the item.|String
|displayName|Display name for this tag. For the default tag, the display name is empty.|String
|id|A string identifier.|String
|image|An image of the item.|[Image](#image)
|name|The name of the thing represented by this object.|String
|readlink|The URL that returns this resource. To use the URL, append query parameters as appropriate and include the Ocp-Apim-Subscription-Key header.|String
|url|The URL to get more information about the thing represented by this object.|String
|webSearchUrl|The URL to Bing's search result for this item.|String


## ImageTagRegion

Defines an image region relevant to the [ImageTag](#imagetag).

|Name|Value|Type
|-|-|-
|displayRectangle|A recommended rectangle to show to the user.|[NormalizedQuadrilateral](#normalizedquadrilateral)
|queryRectangle|A rectangle that outlines the area of interest for this tag.|[NormalizedQuadrilateral](#normalizedquadrilateral)


## InsightsMetadata  

Defines a count of the number of websites where you can shop or perform other actions related to the image.  
  
|Name|Value|Type
|-|-|-
|aggregateOffer|A summary of the online offers of products found in the image. For example, if the image is of a dress, the offer might identify the lowest price and the number of offers found. Only visually similar products insights include this field. The offer includes the following fields: `name`, `aggregateRating`, `offerCount`, and `lowPrice`.|[AggregateOffer](#aggregateoffer)
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


## NormalizedQuadrilateral

Defines a region of an image. The region is a convex quadrilateral defined by coordinates of its top left, top right, bottom left, and bottom right points. The coordinates are fractional values of the original image's width and height in the range 0.0 through 1.0.

|Name|Value|Type
|-|-|-
|_type|A type hint.|String
|alternateName|An alias for the item.|String
|bingId|An ID that uniquely identifies this item.|String
|bottomLeft|The bottom left corner coordinate.|[Point2D](#point2d)
|bottomRight|The bottom right corner coordinate.|[Point2D](#point2d)
|description|A short description of the item.|String
|id|A String identifier.|String
|image|An image of the item.|[Image](#image)
|name|The name of the thing represented by this object.|String
|readLink|The URL that returns this resource. To use the URL, append query parameters as appropriate and include the Ocp-Apim-Subscription-Key header.|String
|topLeft|The top left corner coordinate.|[Point2D](#point2d)
|topRight|The top right corner coordinate.|[Point2D](#point2d)
|url|The URL to get more information about the thing represented by this object.|String
|webSearchUrl|The URL to Bing's search result for this item.|String


## Offer

Defines a merchant's offer.

|Name|Value|Type
|-|-|-
|aggregateRating|An aggregated rating that indicates how well the product has been rated by others.|[AggregateRating](#aggregaterating)
|alternateName|An alias for the item.|String
|availability|The item's availability. The following are the possible values:<ul><li>Discontinued</li><li>InStock</li><li>InStoreOnly</li><li>LimitedAvailability</li><li>OnlineOnly</li><li>OutOfStock</li><li>PreOrder</li><li>SoldOut</li></ul>|String
|bingId|An ID that uniquely identifies this item.|String
|description|A short description of the item.|String
|id|A String identifier.|String
|image|An image of the item.|[Image](#image)
|lastUpdated|The last date that the offer was updated. The date is in the form YYYY-MM-DD.|String
|name|The name of the thing represented by this object.|String
|price|The item's price.|Double
|priceCurrency|The three-letter ISO 4217 currency code that `price` is specified in. For example, USD.|String
|readLink|The URL that returns this resource. To use the URL, append query parameters as appropriate and include the Ocp-Apim-Subscription-Key header.|String
|seller|The seller for this offer.|[Organization](#organization)
|url|The URL to get more information about the thing represented by this object.|String
|webSearchUrl|The URL to Bing's search result for this item.|String


## Organization

Defines an organization.

|Name|Value|Type
|-|-|-
|_type|A type hint.|String
|alternateName|An alias for the item.|String
|bingId|An ID that uniquely identifies this item.|String
|description|A short description of the item.|String
|id|A String identifier.|String
|image|An image of the item.|[Image](#image)
|name|The name of the thing represented by this object.|String
|readLink|The URL that returns this resource. To use the URL, append query parameters as appropriate and include the Ocp-Apim-Subscription-Key header.|String
|url|The URL to get more information about the thing represented by this object.|String
|webSearchUrl|The URL to Bing's search result for this item.|String


## Point2D

Defines a 2-dimensional point with X and Y coordinates.

|Name|Value|Type
|-|-|-
|_type|A type hint.|String
|alternateName|An alias for the item.|String
|bingId|An ID that uniquely identifies this item.|String
|description|A short description of the item.|String
|id|A String identifier.|String
|image|An image of the item.|[Image](#image)
|name|The name of the thing represented by this object.|String
|readLink|The URL that returns this resource. To use the URL, append query parameters as appropriate and include the Ocp-Apim-Subscription-Key header.|String
|url|The URL to get more information about the thing represented by this object.|String
|webSearchUrl|The URL to Bing's search result for this item.|String
|x|The point's x-coordinate.|Integer
|y|The point's y-coordinate.|Integer


## Query  

Defines a search query.  
   
|Name|Value|Type
|-|-|-
|<a name="query-displaytext"></a>displayText|A display version of the query term.|String
|<a name="query-searchurl"></a>searchUrl|The URL that you use to get the results of the related search. Before using the URL, append query parameters as appropriate.<br/><br/>Use this URL if you're displaying the results in your own user interface. Otherwise, use the URL in `webSearchUrl`.|String
|<a name="query-text"></a>text|The query string. Use this string as the query term in a new search request.|String  
|<a name="query-thumbnail"></a>thumbnail|The URL to a thumbnail of a related image. The Image object includes only the `url` field.|[Image](#image) 
|<a name="query-websearchurl"></a>webSearchUrl|The URL that takes the user to the Bing search results page for the query.|String


## Recipe

Defines a cooking recipe.

|Name|Value|Type
|-|-|-
|cookTime|The amount of time the food takes to cook. For example, PT25M. For information about the time format, see <a href="http://en.wikipedia.org/wiki/ISO_8601#Durations" target="_blank">http://en.wikipedia.org/wiki/ISO_8601#Durations</a>.|String
|datePublished|The date that the CreativeWork was published.|String
|prepTime|The amount of time required to prepare the ingredients. For example, PT15M. For information about the time format, see <a href="http://en.wikipedia.org/wiki/ISO_8601#Durations" target="_blank">http://en.wikipedia.org/wiki/ISO_8601#Durations</a>.|String
|provider|The source of the creative work.|[Thing](#thing)
|text|Text content of this creative work.|String
|thumbnailUrl|The URL to a thumbnail of the item.|String
|totalTime|The total amount of time it takes to prepare and cook the recipe. For example, PT45M. For information about the time format, see <a href="http://en.wikipedia.org/wiki/ISO_8601#Durations" target="_blank">http://en.wikipedia.org/wiki/ISO_8601#Durations</a>.|String


## RecipesModule

Defines a list of recipes.

|Name|Value|Type
|-|-|-
|value|A list of recipes.|[Recipe](#recipe)[]


## RelatedSearchesModule

Defines a list of related searches.

|Name|Value|Type
|-|-|-
|value|A list of related searches.|[Query](#query)[]


## Thing

Defines a thing.

|Name|Value|Type
|-|-|-
|alternateName|An alias for the item.|String
|bingId|An ID that uniquely identifies this item.|String
|description|A short description of the item.|String
|id|A String identifier.|String
|image|An image of the item.|[Image](#image)
|name|The name of the thing represented by this object.|String
|readLink|The URL that returns this resource. To use the URL, append query parameters as appropriate and include the Ocp-Apim-Subscription-Key header.|String
|url|The URL to get more information about the thing represented by this object.|String
|webSearchUrl|The URL to Bing's search result for this item.|String

