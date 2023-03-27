---
title: Bing Visual Search API v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Visual Search API may return in the JSON response.
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-visual-search
ms.topic: reference
ms.date: 01/19/2023
ms.author: v-apunnamara
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

## Entity  

Defines an entity such as a person, place, or thing.  
  
|Name|Value|Type
|-|-|-
|bingId|An ID that uniquely identifies this entity.|String  
|contractualRules|A list of rules that you must adhere to if you display the entity. For example, the rules may govern attribution of the entity's description.<br/><br/>The following contractual rules may apply:<ul><li>[LicenseAttribution](#licenseattribution)</li><li>[LinkAttribution](#linkattribution)</li><li>[MediaAttribution](#mediaattribution)</li><li>[TextAttribution](#textattribution)</li></ul>Not all entities include rules. If the entity provides contractual rules, you must abide by them. For more information about using contractual rules, see [Attributing Data](/../../bing-entities-search/concepts/data-attribution).|Object[]
|description|A short description of the entity.|String  
|image|An image of the entity.|[Image](#image)
|name|The entity's name.|String
|webSearchUrl|The URL that takes the user to the Bing search results page for this entity.|String

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
|<a name="image-encodingformat"></a>encodingFormat|The image's MIME type (for example, jpeg).|String
|<a name="image-height"></a>height|The height of the source image, in pixels.|Unsigned Short
|<a name="image-hostpagedisplayurl"></a>hostPageDisplayUrl|The display URL of the webpage that hosts the image.<br/><br/>Use this URL in your user interface to identify the host webpage that contains the image. The URL is not a well-formed and should not be used to access the host webpage. To access the host webpage, use the `hostPageUrl` URL.|String
|<a name="image-hostpageurl"></a>hostPageUrl|The URL of the webpage that includes the image. This URL and `contentUrl` may be the same URL.|String
|<a name="image-id"></a>id|An ID that uniquely identifies this image in the list of images.<br/><br/>The object includes this field only in a Web Search API response. For information about how to use this field, see [Ranking results](../../bing-web-search/rank-results.md) in the Web Search API guide.|String
|<a name="image-imageid"></a>imageId|An ID that uniquely identifies this image.|String
|<a name="image-imageinsightstoken"></a>imageInsightsToken|The token that you use when calling the Visual Search API to get insights about the image.|String
|<a name="image-insightsmetadata"></a>insightsMetadata|A count of the number of websites where you can shop or perform other actions related to the image.<br/><br/>For example, if the image is of an apple pie, this object includes a count of the number of websites where you can buy an apple pie. To indicate the number of offers in your UX, include badging such as a shopping cart icon that contains the count. When the user clicks on the icon, use the `imageInsightsToken` in a Visual Search API request to get the list of websites.|[InsightsMetadata](#insightsmetadata)
|<a name="image-name"></a>name|A title of the image.|String
|<a name="image-thumbnail"></a>thumbnail|The width and height of the thumbnail image (see `thumbnailUrl`).|[MediaSize](#mediasize)
|<a name="image-thumbnailurl"></a>thumbnailUrl|A URL to a thumbnail of the image. For information about resizing the image, see [Resize and crop thumbnail images](../../bing-web-search/resize-and-crop-thumbnails.md).|String
|webSearchUrl|A URL to the Bing search results for this image.|String  
|<a name="image-width"></a>width|The width of the source image, in pixels.|Unsigned Short

## ImageEntityAction  

Defines an entity action.  
  
|Name|Value|Type
|-|-|-  
|\_type|A type hint, which is set to ImageEntityAction.|String  
|actionType|A string representing the type of action. Set to Entity.|String
|data|The entity that Bing recognized in the image.|[Entity](#entity)
|datePublished|The date on which the CreativeWork was published.|String
|displayName|The name of the entity.|String
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
|\_type|A type hint, which is set to ImageKnowledge|String
|id|A String identifier.|String
|image|The image used in this insights request. The object includes the `imageInsightsToken` field only.|[Image](#image)
|tags|A list of visual search tags.|[ImageTag](#imagetag)[]

## ImageModuleAction

Defines an image list action.

|Name|Value|Type
|-|-|-  
|\_type|A type hint, which is set to ImageModuleAction.|String
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
|url|The URI used to perform the action. The object includes this field if `actionType` is Uri, which is typically returned as part of text recognition. The URI can use the:<ul><li>HTTP</li><li>HTTPS protocol for cases when the URI is a URL to a website.</li><li>tel protocol for cases when the URI is a telephone number</li><li>mailto protocol for cases when the URI is an email address.</li></ul>|String

## ImageRelatedSearchesAction

Defines a related search action.

|Name|Value|Type
|-|-|-  
|\_type|A type hint, which is set to ImageRelatedSearchesAction.|String
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
|\_type|A type hint, which is set to ImageShoppingSourcesAction.|String
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
|currentOffset|The offset that represents where the first image in `value` is relative to all images that Bing might return for this query. The object includes this field only for the VisualSearch and ProductVisualSearch action types.
|nextOffset|The offset value that you set the `offset` field to in the **KnowledgeRequest** request object. For information about paging, see [Paging VisualSearch and ProductVisualSearch action types](../how-to/get-insights.md#paging). The object includes this field only for the VisualSearch and ProductVisualSearch action types.|Integer
|totalEstimatedMatches|The estimated number of images that match the query. Use this number along with `count` and `offset` to page the results. The object includes this field only for the VisualSearch and ProductVisualSearch action types.|Long
|value|List of images.|[Image](#image)[]

## ImageTag

Defines an image tag.

|Name|Value|Type
|-|-|-
|actions|Actions within this tag. The following are the possible actions:<ul><li>[ImageEntityAction](#imageentityaction)</li><li>[ImageModuleAction](#imagemoduleaction)</li><li>[ImageRelatedSearchesAction](#imagerelatedsearchesaction)</li><li>[ImageShoppingSourcesAction](#imageshoppingsourcesaction)</li></ul>|Object[]
|alternateName|An alias for the item.|String
|bingId|An ID that uniquely identifies this item.|String
|boundingBox|The bounding box for this tag. The bounding box identifies the area of interest in the image. There is no bounding box for the default tag.|[ImageTagRegion](#imagetagregion)
|description|A short description of the item.|String
|displayName|The display name for this tag. The tag with the empty display name represents the default tag. The default tag contains all insights except for the entity insight. The entity insight is in another tag and its `actionType` is Entity.|String
|id|A string identifier.|String
|image|An image of the item. The object includes the `thumbnailUrl` field only.|[Image](#image)
|name|The name of the thing represented by this object.|String
|readlink|The URL that returns this resource. To use the URL, append query parameters as appropriate and include the Ocp-Apim-Subscription-Key header.|String
|sources|A list of sources used to recognize text in the image. For example, OCR (optical character recognition).|String[]
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
|aggregateOffer|A summary of the online offers of products found in the image. For example, if the image is of a dress, the offer might identify the lowest price and the number of offers found. The offer includes the following fields: `name`, `aggregateRating`, `offerCount`, and `lowPrice`.|[AggregateOffer](#aggregateoffer)
|<a name="availablesizescount"></a>availableSizesCount|The number of different sizes of the image that Bing found on one or more websites.|Unsigned Integer  
|<a name="pagesincludingcount"></a>pagesIncludingCount|The number of webpages that include the image.|Unsigned Integer  
|<a name="recipesourcecount"></a>recipeSourcesCount|The number of websites that offer recipes of the food seen in the image.|Unsigned Integer  
|<a name="shoppingsourcecount"></a>shoppingSourcesCount|The number of websites that offer the products seen in the image.|Unsigned Integer

## ItemRegion

Defines a word within a line of text.

|Name|Value|Type
|-|-|-
|boundingBox|The bounding box of the word.|[NormalizedQuadrilateral](#normalizedquadrilateral)
|text|The word within a line of text.|String

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
|\_type|A type hint, which is set to LicenseAttribution.|String
|license|The license under which the content may be used.|[License](#license)
|licenseNotice|The license to display next to the targeted field. For example, "Text under CC-BY-SA license".<br/><br/>Use the license's name and URL in the `license` field to create a hyperlink to the website that describes the details of the license. Then, replace the license name in the `licenseNotice` string (for example, CC-BY-SA) with the hyperlink you just created.|String
|mustBeCloseToContent|A Boolean value that determines whether the contents of the rule must be placed in close proximity to the field that the rule applies to. If **true**, the contents must be placed in close proximity. If **false**, or this field does not exist, the contents may be placed at the caller's discretion.|Boolean
|targetPropertyName|The name of the field that the rule applies to.|String

## LinkAttribution  

Defines a contractual rule for link attribution.  
  
|Name|Value|Type
|-|-|-
|\_type|A type hint, which is set to LinkAttribution.|String
|mustBeCloseToContent|A Boolean value that determines whether the contents of the rule must be placed in close proximity to the field that the rule applies to. If **true**, the contents must be placed in close proximity. If **false**, or this field does not exist, the contents may be placed at the caller's discretion.|Boolean
|targetPropertyName|The name of the field that the rule applies to.<br/><br/>If a target is not specified, the attribution applies to the entity as a whole and should be displayed immediately following the entity presentation. If there are multiple text and link attribution rules that do not specify a target, you should concatenate them and display them using a "Data from: " label. For example, “Data from <provider name1\> &#124; <provider name2\>".|String
|text|The attribution text.|String
|url|The URL to the provider's website. Use `text` and URL to create of hyperlink.|String

## MediaAttribution  

Defines a contractual rule for media attribution.  
  
|Name|Value|Type
|-|-|-  
|\_type|A type hint, which is set to MediaAttribution.|String
|mustBeCloseToContent|A Boolean value that determines whether the contents of the rule must be placed in close proximity to the field that the rule applies to. If **true**, the contents must be placed in close proximity. If **false**, or this field does not exist, the contents may be placed at the caller's discretion.|Boolean
|targetPropertyName|The name of the field that the rule applies to.|String
|url|The URL that you use to create of hyperlink of the media content. For example, if the target is an image, you would use the URL to make the image clickable.|String  

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
|\_type|A type hint.|String
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
|\_type|A type hint.|String
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
|\_type|A type hint.|String
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

## RelatedSearchesModule

Defines a list of related searches.

|Name|Value|Type
|-|-|-
|value|A list of related searches.|[Query](#query)[]

## TextAttribution  

Defines a contractual rule for plain text attribution.  
  
|Name|Value|Type
|-|-|-
|\_type|A type hint, which is set to TextAttribution.|String
|text|The attribution text.<br/><br/>Text attribution applies to the entity as a whole and should be displayed immediately following the entity presentation. If there are multiple text or link attribution rules that do not specify a target, you should concatenate them and display them using a "Data from: " label.|String

## TextRecognitionAction

Defines a text recognition action.

|Name|Value|Type
|-|-|-  
|\_type|A type hint, which is set to ImageKnowledge/TextRecognitionAction.|String
|actionType|A string representing the type of action, which is set to TextRecognition.|String
|data|The recognized text found in the image.|[TextRegionsModule](#textregionsmodule)

## TextLine

Defines a line of text that was found in the image.

|Name|Value|Type
|-|-|-
|boundingBox|The bounding box within which the line of text was found.|[NormalizedQuadrilateral](#normalizedquadrilateral)
|text|The line of text.|String
|words|A list of words within the line of text.|[ItemRegion](#itemregion)[]

## TextRegion

Defines an area where text was found in the image.

|Name|Value|Type
|-|-|-
|boundingBox|The bounding box within which the lines of text was found.|[NormalizedQuadrilateral](#normalizedquadrilateral)
|lines|A list of areas within the bounding box where lines of text was found.|[TextLine](#textline)[]

## TextRegionsModule

Defines the list of areas where text was found in the image.

|Name|Value|Type
|-|-|-
|boundingBox|The bounding box within which all text in the image was found.|[NormalizedQuadrilateral](#normalizedquadrilateral)
|regions|A list of areas within the bounding box where text was found.|[TextRegion](#textregion)[]

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
