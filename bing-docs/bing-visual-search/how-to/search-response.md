---
title: Handle the Visual Search response
titleSuffix: Bing Search Services
description: Shows what the different image insights look like in the JSON response.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 7/15/2020
ms.author: scottwhi
---

# Handle the visual search response

If Bing found insights about an image that you sent Visual Search API, the response contains the following structure.

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {...},
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

The `image` field is an [Image](../reference/respons-objects.md#image) object that includes only the insights token of the image used in the request.

The `tags` field contains an array of [ImageTag](../reference/respons-objects.md#imagetag) objects. Each tag contains a list of actions, display name, an optional bounding box.

The tag you're probably most interested in is the one where the display name is an empty string. This tag contains insights about the image used in the request, such as webpages that include the image, visually similar images, shopping sources and more. Because the entire image is of interest, the default insights tag doesn't include bounding boxes for the regions of interest. [Read more](#default-insights-tag).


```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {
      "displayName" : "",
      "actions" : [
        {...},
        {...},
        {...},
        {...}
      ]
    },
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

The other tags contain a diverse set of terms derived from the input image. These terms enable users to explore concepts found in the image. For example, if the image used in the request shows a black, long sleeve t-shirt, the other tags may contain URLs that you can use to find images of related clothing, such as jackets or sweatshirts. [Read more](#other-tags).

[!INCLUDE [bing-url-note](../../../includes/bing-url-note.md)]


## Default insights tag

The default tag is the one where the `displayName` field is set to an empty string. This tag provides insights about the image that you sent in the request. The `actionType` field identifies the insight. The list of actions the response includes depends on the image. And for each action, the list of properties may vary by image, so check if the property exists before trying to use it.


The following example shows the possible list of insights. 

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {
      "displayName" : "",
      "actions" : [
        {
          "_type" : "ImageModuleAction",
          "actionType" : "PagesIncluding",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageShoppingSourcesAction",
          "actionType" : "ShoppingSources",
          "data" : {
            "offers" : [...]
          }
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "VisualSearch",
          "data" : {
            "value" : [...]
          }
        },
        {
          "image" : {...},
          "actionType" : "ImageById"
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "ProductVisualSearch",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageRelatedSearchesAction",
          "actionType" : "RelatedSearches",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageRelatedSearchesAction",
          "actionType" : "DocumentLevelSuggestions",
          "data" : {
            "value" : [...]
          }
        },
        {
          "actionType" : "BestRepresentativeQuery",
          "webSearchUrl" : "",
          "displayName" : "",
          "serviceUrl" : ""
        }
      ]
    },
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

### PagesIncluding insight

The PagesIncluding insight provides a list of webpages that include this image. It's actually a list of **Image** objects and the `hostPageUrl` field contains the URL to the webpage that includes the image.

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "PagesIncluding",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https://www.bing.com/images/search?",
              "name" : "Today's smoking hot country",
              "thumbnailUrl" : "https://tse2.mm.bing.net/th?id=OIP...",
              "datePublished" : "2017-09-20T12:00:00.0000000Z",
              "isFamilyFriendly": true,
              "contentUrl" : "http://contoso.com/wordstuff",
              "hostPageUrl" : "http://contoso.com/2017/09/20/car",
              "contentSize" : "122540 B",
              "encodingFormat" : "jpeg",
              "hostPageDisplayUrl" : "contoso.com/2017/09/20/car",
              "width" : 894,
              "height" : 1200,
              "thumbnail" : {
                "width" : 474,
                "height" : 636
              },
              "imageInsightsToken" : "ccid_CO5GEthj*mid_5323B1",
              "insightsMetadata" : {
                "pagesIncludingCount" : 12,
                "availableSizesCount" : 7
              },
              "imageId" : "5323B1900FB9087B6B45D176D234E1F2F23CD3A5",
              "accentColor" : "55585B"
            }
          ]
        }
      }
```

If Bing didn’t find any webpages that include the image, the value field contains an empty array.

```json
      "actions": [
        {
          "_type": "ImageModuleAction",
          "actionType": "PagesIncluding",
          "data": {
            "value": []
          }
        },
```

### ShoppingSources insight

The ShoppingSources insight provides a list of websites where the user can buy the item seen in the image. Each offer provides the item’s pricing, rating, and the URL of the webpage where the user can buy the item.

```json
      {
        "_type" : "ImageShoppingSourcesAction",
        "actionType" : "ShoppingSources",
        "data" : {
          "offers" : [
            {
              "name" : "Apple Pie",
              "url" : "https://contoso.com/product_p/l10.htm",
              "description" : "A taste of the crust, apple, and pie filling...",
              "seller" : {
                "name" : "Contoso",
                "image": {
                  "url": "https://tse2.mm.bing.net/th?id=A309ad75c6afafc7dd..."
                }
              },
              "price" : 3.99,
              "priceCurrency" : "USD",
              "availability": "InStock",
              "aggregateRating" : {
                "ratingValue" : 5
              },
              "lastUpdated" : "2018-04-16T00:00:00.0000000"
            }
          ]
        }
      }
```

The default tag doesn’t include this insight if Bing didn’t find any shopping sources. If you passed an image insights token, Bing only includes the shopping insight if the image's `shoppingSourcesCount` field is greater than zero.

```json
      "imageInsightsToken": "ccid_7qgerj5g*cp_96BBF7CD1AD0AE8E77A92...",
      "insightsMetadata": {
        "shoppingSourcesCount": 1,
        "recipeSourcesCount": 0,
        "aggregateOffer": {
          "name": "Little Black Party Dress...",
          "priceCurrency": "USD",
          "lowPrice": 20.99,
          "offerCount": 1
        },
        "pagesIncludingCount": 11,
        "availableSizesCount": 4
      },
```

### VisualSearch insight

The VisualSearch insight provides a list of images that are visually similar to the original image (contains content that's similar to the content seen in the original image).

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "VisualSearch",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https://www.bing.com/images/search?view=...",
              "name" : "An apple pie...",
              "thumbnailUrl" : "https://tse4.mm.bing.net/th?id=OIP.z...",
              "datePublished" : "2017-03-18T00:17:00.0000000Z",
              "contentUrl" : "http://contoso.net/images/8/8a/an_apple_pie.png",
              "hostPageUrl" : "http://contoso.com/wiki/an_apple_pie.png",
              "contentSize" : "87930 B",
              "encodingFormat" : "png",
              "hostPageDisplayUrl" : "contoso.com/wiki/an_apple_pie.png",
              "width" : 263,
              "height" : 192,
              "thumbnail" : {
                "width" : 474,
                "height" : 346
              },
              "imageInsightsToken" : "ccid_zhRxfGkI*mid_1DCBA7AA6D231...",
              "insightsMetadata" : {
                "recipeSourcesCount" : 6,
                "pagesIncludingCount" : 103,
                "availableSizesCount" : 28
              },
              "imageId" : "1DCBA7AA6D23147F9DD06D47DB3A38EB25389",
              "accentColor" : "3E0D01"
            }
          ]
        }
      }
```

### ImageById insight

The ImageById insight provides an **Image** object of the source image.

```json
      {
        "image" : {
          "webSearchUrl" : "https://www.bing.com/images/search?view=deta...",
          "name" : "Making Apple Pie",
          "thumbnailUrl" : "https://tse4.mm.bing.net/th?id=OIP...",
          "datePublished" : "2013-06-21T12:00:00.0000000Z",
          "contentUrl" : "http://contoso.com/content/uploads/2013/06/apple-pie.jpg",
          "hostPageUrl" : "http://contoso.com/2013/06/21/making-apple-pie/",
          "contentSize" : "134847 B",
          "encodingFormat" : "jpeg",
          "hostPageDisplayUrl" : "contoso.com/2013/06/21/making-apple-pie",
          "width" : 1050,
          "height" : 765,
          "thumbnail" : {
            "width" : 474,
            "height" : 345
          },
          "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CFE...",
          "insightsMetadata" : {
            "recipeSourcesCount" : 6,
            "pagesIncludingCount" : 103,
            "availableSizesCount" : 28
          },
          "imageId" : "D12339146CFEDF3D409A66D2C98D0D71904D4",
          "accentColor" : "3A0B01"
        },
        "actionType" : "ImageById"
      },
```

### RelatedSearches insight

The RelatedSearches insight provides a list of related searches made by others (based on other users' search terms). Use the `displayText` and `webSearchUrl` fields to create a link that takes the user to Bing’s image search results page for the query string. If you want to provide your own experience, use the URL in `searchLink` to get and display the images yourself. 

```json
      {
        "_type" : "ImageRelatedSearchesAction",
        "actionType" : "RelatedSearches",
        "data" : {
          "value" : [
            {
              "text" : "Homemade Apple Pies Recipes",
              "displayText" : "Homemade Apple Pies Recipes",
              "webSearchUrl" : "https://www.bing.com/images/search?q=Homemade...",
              "searchLink": "https://api.bing.microsoft.com/api/v7/images/search?q=Homemade...",
              "thumbnail" : {
                "url" : "https://tse1.mm.bing.net/th?q=Homemade+Apple+Pies"
              }
            }
          ]
        }
      }
```

### DocumentLevelSuggestions insight

The DocumentLevelSuggestions insight provides a list of suggested search terms based on the contents of the image. Use the `displayText` and `webSearchUrl` fields to create a link that takes the user to Bing’s image search results page for the query string.

```json
      {
        "_type" : "ImageRelatedSearchesAction",
        "actionType" : "DocumentLevelSuggestions",
        "data" : {
          "value" : [
            {
              "text" : "American Apple Pie",
              "displayText" : "American Apple Pie",
              "webSearchUrl" : "https://www.bing.com/images/search?q=American",
              "thumbnail" : {
                "url" : "https://tse3.mm.bing.net/th?q=American+Apple+Pie..."
              }
            }
          ]
        }
      }
```

### BestRepresentativeQuery insight

The BestRepresentativeQuery insight provides the query string that Bing thinks best represents the image’s subject (see the `displayName` field). You can use the `webSearchUrl` URL to send the user to Bing’s image search results page or you can use the `serviceUrl` URL to get the images yourself and display them. If you use the `serviceUrl` URL, remember to append query parameters as appropriate. 

```json
        {
          "webSearchUrl": "https://www.bing.com/images/search?q=Black+Tea+Length+...",
          "webSearchUrlPingSuffix": "DevEx,5850.1",
          "displayName": "Black Tea Length Cocktail Dresses",
          "serviceUrl": "https://api.bing.microsoft.com/v7.0/images/search?q=Black+Te...",
          "actionType": "BestRepresentativeQuery"
        },
```

### ProductVisualSearch insight

The ProductVisualSearch insight provides a list of images that contain products that are visually similar to products seen in the original image. The `aggregateOffer` field in the **InsightsMetadata** object contains offers where you can buy the product and the price of the product. Not all images in the list include offers.

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "ProductVisualSearch",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https://www.bing.com/images/search?view=detail...",
              "name" : "Contoso 4-Piece Kitchen Package...",
              "thumbnailUrl" : "https://tse3.mm.bing.net/th?id=OIP.l9hza...",
              "datePublished" : "2017-07-16T04:28:00.0000000Z",
              "contentUrl" : "https://www.contoso.com/assets/media/images/prod...",
              "hostPageUrl" : "https://www.contoso.com/4-piece-kitchen-package...",
              "contentSize" : "13594 B",
              "encodingFormat" : "jpeg",
              "hostPageDisplayUrl" : "https://www.contoso.com/4-piece-kitchen...",
              "width" : 450,
              "height" : 332,
              "thumbnail" : {
                "width" : 474,
                "height" : 349
              },
              "imageInsightsToken" : "ccid_l9hzaabu*mid_70A8B616355D681DB9A5A...",
              "insightsMetadata" : {
                "shoppingSourcesCount" : 1,
                "recipeSourcesCount" : 0,
                "aggregateOffer" : {
                  "name":"4-Piece Kitchen Package with...",
                  "priceCurrency":"USD",
                  "lowPrice":2756,
                  "offers" : [
                    {
                      "name" : "4-Piece Kitchen Package with...",
                      "url" : "https://www.fabrikam.com/1234.html?ref=bing",
                      "description" : "This 36 Frenchdoor refrigerator by...",
                      "seller" : {
                        "name" : "Fabrikam",
                        "image" : {
                          "url" : "https://tse1.mm.bing.net/th?id=A818f811..."
                        }
                      },
                      "price" : 2756,
                      "priceCurrency" : "USD",
                      "availability" : "InStock",
                      "lastUpdated" : "2018-02-20T00:00:00.0000000"
                    }
                  ],
                  "offerCount":1
                },
                "pagesIncludingCount" : 4,
                "availableSizesCount" : 2
              },
              "imageId" : "70A8B616355D681DA5980A8D0514BCC995A3",
              "accentColor" : "60646B"
            }
          ]
        }
      }
```


## Entity insight tag

If Bing recognizes an entity that is a culturally well-known person, place, or thing, one or more of the tags might include an Entity action. Use the `displayName` and `webSearchUrl` fields to create a hyperlink that takes the user to Bing’s search results where they can learn more about the entity.

```json
    {
      "image": {
        "thumbnailUrl": "https://tse1.mm.bing.net/th?q=Statue+of+Liberty&pid=Api..."
      },
      "displayName": "Statue of Liberty",
      "boundingBox": { . . . },
      "actions": [
        {
          "_type": "ImageEntityAction",
          "webSearchUrl": "https://www.bing.com/search?q=Statue+of+Liberty",
          "displayName": "Statue of Liberty",
          "actionType": "Entity"
        },
        {
          "webSearchUrl": "https://www.bing.com/search?q=Statue+of+Liberty",
          "displayName": "Statue of Liberty",
          "serviceUrl": "https://api.bing.microsoft.com/v7.0/search?q=Statue+of+Liberty",
          "actionType": "TextResults"
        },
        {
          "webSearchUrl": "https://www.bing.com/images/search?q=Statue+of+Liberty",
          "displayName": "Statue of Liberty",
          "serviceUrl": "https://api.bing.microsoft.com/v7.0/images/search?q=Statue+of+Liberty",
          "actionType": "ImageResults"
        }
      ]
    },
``` 

But if you set the `enableEntityData` field to **true** in your request, the Entity action includes a `data` that contains the entity data that you can display yourself. For information about setting `enableEntityData`, see [Including entity data](get-insights.md#including-entity-data).

```json
    {
      "image": {
        "thumbnailUrl": "https://tse4.mm.bing.net/th?q=Bill+Gates&pid=Api&mkt=en-US&adlt=moderate"
      },
      "displayName": "Bill Gates",
      "boundingBox": { ... },
      "actions": [
        {
          "_type": "ImageEntityAction",
          "webSearchUrl": "https://www.bing.com/search?q=Bill+Gates",
          "mainEntity": {
            "bingId": "0d47c987-0042-5576-15e8-97af601614fa"
          },
          "displayName": "Bill Gates",
          "actionType": "Entity",
          "data": {
            "id": "https://api.bing.microsoft.com/v7.0/entities/0d47c987-0042-5576-15e8-97af601614fa",
            "readLink": "https://api.bing.microsoft.com/v7.0/search?q=Bill+Gates&filters=sid:%220d47c987-0042-5576-15e8-97af601614fa%22&responsefilter=entities,places",
            "contractualRules": [
              {
                "_type": "ContractualRules/LicenseAttribution",
                "targetPropertyName": "description",
                "mustBeCloseToContent": true,
                "license": {
                  "name": "CC-BY-SA",
                  "url": "http://creativecommons.org/licenses/by-sa/3.0/"
                },
                "licenseNotice": "Text under CC-BY-SA license"
              },
              {
                "_type": "ContractualRules/LinkAttribution",
                "targetPropertyName": "description",
                "mustBeCloseToContent": true,
                "text": "Wikipedia",
                "url": "http://en.wikipedia.org/wiki/Bill_Gates"
              }
            ],
            "webSearchUrl": "https://www.bing.com/entityexplore?q=Bill+Gates&filters=sid:...",
            "name": "Bill Gates",
            "image": {
              "thumbnailUrl": "https://tse3.mm.bing.net/th?id=AMMS_be3b66a21b09224ad5e8...",
              "hostPageUrl": "http://upload.wikimedia.org/wikipedia/commons/1/19/Bill_Gates_June_2015.jpg",
              "width": 50,
              "height": 50,
              "sourceWidth": 474,
              "sourceHeight": 670
            },
            "description": "William Henry Gates III is an American business magnate, software developer...",
            "bingId": "0d47c987-0042-5576-15e8-97af601614fa"
          }
        },
        {
          "webSearchUrl": "https://www.bing.com/images/search?q=Bill+Gates",
          "webSearchUrlPingSuffix": "DevEx,6215.1",
          "displayName": "Bill Gates",
          "serviceUrl": "https://api.bing.microsoft.com/v7.0/images/search?q=Bill+Gates",
          "actionType": "ImageResults"
        }
      ]
    },
```


## The other tags

The other tags contain a diverse set of terms derived from the image sent in the request. These terms enable users to explore concepts found in the image. For example, if the image used in the request shows a black, long sleeve t-shirt, the other tags may contain URLs that you can use to find images of related clothing, such as jackets or sweatshirts. 

Each tag includes a display name that you can use to categorize the related images, a bounding box that identifies the region of interest in the image, a thumbnail of the image that represents the related images, and URLs that you can use to explore the derived term. 

For information about the bounding boxes, see [Using bounding boxes](#using-bounding-boxes).

In this example, the list of actions includes the TextResults action type and the ImageResults action type. The TextResults action contains the URLs used to get web search results and the ImageResults action contains the URLs used to get image search results. Use the `serviceUrl` URL to call the Image Search API or Web Search API and display the search results yourself. Otherwise, you can use the `webSearchUrl` URL to send the user to Bing's search results page.

```json
    {
      "image": {
        "thumbnailUrl": "https://tse3.mm.bing.net/th?q=sweatshirt&pid=..."
      },
      "displayName": "sweatshirt",
      "boundingBox": {...},
      "actions": [
        {
          "webSearchUrl": "https://www.bing.com/search?q=sweatshirt+sweater",
          "displayName": "sweatshirt sweater",
          "serviceUrl": "https://api.bing.microsoft.com/v7.0/search?q=sweatshirt+sweater",
          "actionType": "TextResults"
        },
        {
          "webSearchUrl": "https://www.bing.com/images/search?q=sweatshirt",
          "displayName": "sweatshirt",
          "serviceUrl": "https://api.bing.microsoft.com/v7.0/images/search?q=sweatshirt",
          "actionType": "ImageResults"
        }
      ]
    },
```


## Using bounding boxes

All tags other than the default insights tag include bounding boxes that identify regions of interest in the image. For example, if the image includes several celebrities, the tags could include bounding boxes for each recognized celebrity, or if the image contains recognized articles of clothing, the tags could include bounding boxes for each recognized article of clothing. 

You can use the bounding boxes to create hot spots over the image. When the user hovers over the hotspot, you could display details about the contents in that region of the image. You should not include hot spots in an image for bounding boxes that identify the entire image.

```json
    {
      "image": {
        "thumbnailUrl": "https://tse4.mm.bing.net/th?q=prom+dress..."
      },
      "displayName": "prom dress",
      "boundingBox": {
        "queryRectangle": {
          "topLeft": { "x": 0.83698124, "y": 0.13108893 },
          "topRight": { "x": 0.94547665, "y": 0.13108893 },
          "bottomRight": { "x": 0.94547665, "y": 0.24928382 },
          "bottomLeft": { "x": 0.83698124, "y": 0.24928382 }
        },
        "displayRectangle": {
          "topLeft": { "x": 0.8912289, "y": 0.19018638 },
          "topRight": { "x": 0.8912289, "y": 0.19018638 },
          "bottomRight": { "x": 0.8912289, "y": 0.19018638 },
          "bottomLeft": { "x": 0.8912289, "y": 0.19018638 }
        }
      },
      "actions": [
        {
          "webSearchUrl": "https://www.bing.com/search?q=dress+for+prom",
          "displayName": "dress for prom",
          "serviceUrl": "https://api.bing.microsoft.com/v7.0/search?q=dress+for+prom",
          "actionType": "TextResults"
        },
        {
          "webSearchUrl": "https://www.bing.com/images/search?q=prom+dress",
          "displayName": "prom dress",
          "serviceUrl": "https://api.bing.microsoft.com/v7.0/images/search?q=prom+dress",
          "actionType": "ImageResults"
        }
      ]
    },
```

Typically, you use the `queryRectangle` coordinates to outline the area of interest or the `displayRectangle` coordinates to create a hotspot over the middle of the area of interest.

Use the `webSearchUrl` URL to send the user to Bing’s image search results page where they can see more images like the one seen in the query rectangle. Or, if you want to display the images in your UX, use the `serviceUrl` URL to get the images. If you use the `serviceUrl` URL, remember to add query parameters as appropriate.

The following example shows what the bounding rectangles look like if the area of interest is the entire image.

```json
      "boundingBox": {
        "queryRectangle": {
          "topLeft": { "x": 0, "y": 0 },
          "topRight": { "x": 1, "y": 0 },
          "bottomRight": { "x": 1, "y": 1 },
          "bottomLeft": { "x": 0, "y": 1 }
        },
        "displayRectangle": {
          "topLeft": { "x": 0, "y": 0 },
          "topRight": { "x": 1, "y": 0 },
          "bottomRight": { "x": 1, "y": 1 },
          "bottomLeft": { "x": 0, "y": 1 }
        }
      },
```



<!--
### Text recognition

If the image contains text that the service recognizes, one of the tags will contain a TextResults insight (action). The insight's `displayName` contains the recognized text:

```json
    {
        "image" : {
            "thumbnailUrl" : "https://tse3.mm.bing.net/th?q=%23%23Text..."
        },
        "displayName" : "##TextRecognition",
        "boundingBox" : {
            "queryRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            },
            "displayRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            }
        },
        "actions" : [{
            "displayName" : "WALK BIKE ACROSS BRIDGE",
            "actionType" : "TextResults"
        }],
        "sources" : ["OCR"]
    }
```

Because the tag's `displayName` field contains ##TextRecognition, do not use it as a category title in the UX. That goes for any display name that starts with ##. Instead, use the action's display name.

Text recognition can also recognize the contact information on business cards, such as phone numbers and email addresses. The bounding box identifies the location of the contact information on the card.

```json
    {
      "image" : {
        "thumbnailUrl" : "https://tse3.mm.bing.net/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        }
      },
      "actions" : [
        {
          "url" : "tel:888%20555%201212",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https://tse3.mm.bing.net/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        }
      },
      "actions" : [
        {
          "url" : "mailto:someone@outlook.com",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https://tse3.mm.bing.net/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        }
      },
      "actions" : [
        {
          "displayName" : "CHARLENE WHITNEY Graphic Designer 888 555 1212 someone@outlook.com www.contoso.com",
          "actionType" : "TextResults"
        }
      ],
      "sources" : ["OCR"]
    }
```
-->

## Next steps

- Learn about [use and display requirements](../bing-web-search/use-display-requirements.md) for Bing Visual Search.  
- Learn about how to [get image insights](how-to/get-insights.md).
- Learn about the [quickstarts](../quickstarts/quickstarts.md) and [samples](../samples.md) that are available to help you get up and running fast.
- Review [Visual Search API v7 reference](reference/endpoints.md) documentation.  





















