---
title: "Sending search queries to the Bing Visual Search API"
titleSuffix: Bing Search Services
description: This article describes the parameters and attributes of requests sent to the Bing Visual Search API, as well as the response object.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Sending search queries to the Bing Visual Search API

This article describes the parameters and attributes of requests sent to the Bing Visual Search API, as well as the response object. 

You can get insights about an image in three ways:

- Using an insights token that you get from an image in a previous call to one of the [Bing Image Search API](../../bing-image-search/reference/endpoints.md) endpoints.
- Sending the URL of an image.
- Uploading an image (in binary format).

## Bing Visual Search requests

If you send Visual Search an image token or URL, the following snippet shows the JSON object that you must include in the body of the POST:

```json
{
    "imageInfo" : {
        "url" : "",
        "imageInsightsToken" : "",
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.5,
            "right" : 0.9,
            "bottom" : 0.9
        }
    },
    "knowledgeRequest" : {
      "filters" : {
        "site" : ""
      }
    }
}
```

The `imageInfo` object must include either the `url` or `imageInsightsToken` field but not both. Set the `url` field to the URL of an Internet-accessible image. The maximum supported image size is 1 MB.

The `imageInsightsToken` must be set to an insights token. To get an insights token, call the Bing Image API. The response contains a list of `Image` objects. Each `Image` object contains an `imageInsightsToken` field, which contains the token.

The `cropArea` field is optional. The crop area specifies the top-left corner and bottom-right corner of a region of interest. Specify the values in the range 0.0 through 1.0. The values are a percentage of the overall width or height. For example, the above example marks the right half of the image as the region of interest. Include it if you want to limit the insights request to the region of interest.

The `filters` object contains a site filter (see the `site` field) that you can use to restrict the similar images and similar products results to a specific domain. For example, if the image is of a Surface Book, you can set `site` to `www.microsoft.com`.

If you want to get insights about a local copy of an image, upload the image as binary data.

For details about including these options in the body of the POST, see [Content form types](#content-form-types).

### Search endpoint

The Visual Search endpoint is: https:\/\/api.bing.microsoft.com/bing/v7.0/images/visualsearch.

Requests must be sent as HTTP POST requests only.


<a name="content-form-types"></a>

### Content form types

Each request must include the `Content-Type` header. The header must be set to: `multipart/form-data; boundary=\<boundary string\>`, where \<boundary string\> is a unique, opaque string that identifies the boundary of the form data. For example, `boundary=boundary_1234-abcd`.

If you send Visual Search an image token or URL, the following snippet shows the form data you must include in the body of the POST. The form data must include the `Content-Disposition` header and you must set its `name` parameter to "knowledgeRequest". For details about the `imageInfo` object, see the request.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "url" : "https://contoso.com/2018/05/fashion/red.jpg"
    }
}

--boundary_1234-abcd--
```

You can optionally set the `enableEntityData` attribute in the header to `true` for detailed information on the main entity in the image you upload, including links to the web and attribution information. This field is `false` by default.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
  "imageInfo" : {
      "url" : "https://contoso.com/2018/05/fashion/red.jpg"
  },
  "knowledgeRequest" : {
    "invokedSkillsRequestData" : {
        "enableEntityData" : "true"
    }
  }
}

--boundary_1234-abcd--
```

If you upload a local image, the following snippet shows the form data you must include in the body of the POST. The form data must include the `Content-Disposition` header. Its `name` parameter must be set to "image" and the `filename` parameter may be set to any string. The `Content-Type` header may be set to any commonly used image mime type. The contents of the form is the binary data of the image. The maximum image size you may upload is 1 MB. The largest of the width or height should be 1,500 pixels or less.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

The following snippet shows how to specify the region of interest of an uploaded image:

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "cropArea" : {
            "top" : 0.2,
            "left" : 0.3,
            "bottom" : 0.7,
            "right" : 0.6
        }
    }
}

--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="image"
Content-Type: image/jpeg


ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

### Example request

The following snippet shows a complete image insights request that passes an image token and region of interest. You get the insights token from a previous call to /images/search:

```  
POST https://api.bing.microsoft.com/bing/v7.0/images/visualsearch?mkt=en-us HTTP/1.1  
Content-Type: multipart/form-data; boundary=boundary_1234-abcd
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com 

--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "imageInsightsToken" : "mid_D6426898706EC7..."
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.2,
            "bottom" : 0.7,
            "right" : 0.5
        }
    }
}

--boundary_1234-abcd--
```

## Bing Visual Search responses


[!INCLUDE [bing-url-note](../../../includes/bing-url-note.md)]

If there are insights available for the image, the response contains one or more `tags` that contain the insights. The `image` field contains the insights token for the input image:

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

The `tags` field contains a display name and list of actions (insights). One of the tags contains a `displayName` field that is set to an empty string. This tag contains the default insights such as webpages that include the image, visually similar images, and shopping sources for items found in the image. Because the entire image is of interest, the default insights tag doesn't include bounding boxes for the regions of interest:

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

For a list of the default insights, see [Default insights tag](../default-insights-tag.md).

The remaining tags contain other insights that may be of interest to the user. For example, if the image contains text, one of the tags may include a TextResults insight, which contains the recognized text. Or, if Bing recognizes an entity (that is, a culturally well-known/popular person, place, or thing) in the image, one of the tags may identify the entity. Visual Search also returns a diverse set of terms (tags) derived from the input image. These tags enable users to explore concepts found in the image. For example, if the input image is of a famous athlete, one of the tags might be Sports, which contains links to images of sports.

Each tag includes a display name that you can use to categorize the insight, bounding box that identifies the region of interest that the insight applies to, the insights themselves, and a thumbnail of the image. For example, if the image is of a person wearing a sports jersey, one of the tags might include a bounding box that bounds the jersey and includes VisualSearch and ProductVisualSearch insights. And another tag might include an ImageResults insight that contains a URL for an /images/search API request to get images that are topically related or a Bing.com search URL that takes the user to the Bing.com image search results.

All tags other than the default insights tag include bounding boxes that identify regions of interest in the image. For example, if the image includes multiple recognized people, tags could include bounding boxes for each of the people, or if the image contains recognized clothing items, tags could include bounding boxes for each recognized clothing item. You can use the bounding boxes to create hot spots over the image that when clicked, provide details about the contents in that region of the image. You should not include hot spots in an image for bounding boxes that identify the entire image.

### Text recognition

If the image contains text that the service recognizes, one of the tags will contain a TextResults insight (action). The insight's `displayName` contains the recognized text:

```json
    {
        "image" : {
            "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23Text..."
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
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
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
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
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
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
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

If the image contains a recognized entity such as a culturally well-known/popular person, place, or thing, one of the tags may include an Entity insight. The `mainEntity` and `data` fields are only available if the `enableEntityData` attribute in the `Content-Type` header is set to `true`.

```json
{
  "image" : {
    "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Statue+of+Liberty..."
  },
  "displayName" : "Statue of Liberty",
  "boundingBox" : {
    "queryRectangle" : {
      "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
      "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
      "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
      "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
    },
    "displayRectangle" : {
      "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
      "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
      "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
      "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
    }
  },
  "actions" : [
    {
      "_type" : "ImageEntityAction",
      "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Statue+of+Liberty",
      "displayName" : "Statue of Liberty",
      "actionType" : "Entity",
      "mainEntity" : {
        "name" = "Statue of liberty",
        "bingId" : "..."
      },
      "data" : {
        "id" : "https://api.cognitive.microsoft.com/api/v7/entities/...",
        "readLink": "https://www.bingapis.com/api/v7/search?q=...",
        "readLinkPingSuffix": "...",
        "contractualRules": [
          {
            "_type": "ContractualRules/LicenseAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "license": {
                "name": "CC-BY-SA",
                "url": "http://creativecommons.org/licenses/by-sa/3.0/",
                "urlPingSuffix": "..."
            },
            "licenseNotice": "Text under CC-BY-SA license"
          },
          {
            "_type": "ContractualRules/LinkAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "text": "Wikipedia",
            "url": "http://en.wikipedia.org/wiki/...",
            "urlPingSuffix": "..."
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q=...",
        "webSearchUrlPingSuffix": "...",
        "name": "Statue of Liberty",
        "image": {
          "thumbnailUrl": "https://tse1.mm.bing.net/th?id=...",
          "hostPageUrl": "http://upload.wikimedia.org/wikipedia/...",
          "hostPageUrlPingSuffix": "...",
          "width": 50,
          "height": 50,
          "sourceWidth": 474,
          "sourceHeight": 598
        },
        "description" : "...",
        "bingId": "..."
        }
      }
  ]
}
```

## See also

- [What is the Bing Visual Search API?](../overview.md)
- [Tutorial: Create a Visual Search single-page web app](../tutorial/visual-search-single-page-app.md)
