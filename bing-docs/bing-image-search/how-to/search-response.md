---
title: Image Search API response structure 
titleSuffix: Bing Search Services
description: Learn how to handle the image search response.
services: bing-search-services
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: conceptual
author: alekhyasasi
ms.date: 03/07/2023
---

# Handle the image search response

When you send a request to Image Search API, it returns an [ImageAnswer](../reference/response-objects.md#imageanswer) object in the response body. The object may include one or more of the following fields:

```json
{
  "_type": "Images",
  "readLink": "images/search?q=black cocktail dresses",
  "webSearchUrl": "https://www.bing.com/images/search?q=black cocktail dresses&FORM=OIIARP",
  "queryContext": { ... },
  "totalEstimatedMatches": 835,
  "nextOffset": 36,
  "currentOffset": 0,
  "value": [ { ... } ],
  "queryExpansions": [ { ... } ],
  "pivotSuggestions": [ { ... } ],
  "relatedSearches": [ { ... } ]
}
```

But if an error occurs, the response body contains an [ErrorResponse](../reference/response-objects.md#errorresponse) object. Bing returns an error response for all 400 level HTTP status codes. [Read more](../reference/error-codes.md).

```json
{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidAuthorization", 
      "subCode": "AuthorizationMissing", 
      "message": "Authorization is required.", 
      "moreDetails": "Subscription key is not recognized."
    }
  ]
}
```

For information about the `nextOffset` and `totalEstimatedMatches` fields, see [Paging image and video results](../../bing-web-search/page-results.md#paging-image-and-video-results).

> [!NOTE]
>
> - Images must be displayed in the order provided in the response.
> - Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.

The `value` field contains the list of [Image](../reference/response-objects.md#image) objects that Bing thought were relevant. Here's what the **Image** object looks like in the response.

```json
{
  "value": [
    {
      "webSearchUrl": "https://www.bing.com/images/search?view=detailv2&FORM=OIIRPO...",
      "name": "The Perfect Black Cocktail Dress...",
      "thumbnailUrl": "https://tse4.mm.bing.net/th?id=OIP.fGuCgUtRUl_f2c8...",
      "datePublished": "2014-11-02T12:00:00.0000000Z",
      "isFamilyFriendly": true,
      "contentUrl": "http://contoso.com/wp-content/uploads/2014/11/black...",
      "hostPageUrl": "http://contoso.com/2014/11/02/the-perfect-black-cocktail-dress...",
      "contentSize": "171202 B",
      "encodingFormat": "jpeg",
      "hostPageDisplayUrl": "contoso.com/2014/11/02/the-perfect-black...",
      "width": 996,
      "height": 1500,
      "thumbnail": {
        "width": 474,
        "height": 713
      },
      "imageInsightsToken": "ccid_fGuCgUtR*cp_D72F7E52B27BF10...",
      "insightsMetadata": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0,
        "pagesIncludingCount": 31,
        "availableSizesCount": 15
      },
      "imageId": "1A8B199F1D3ED20462C904A1F5353C57736A2D44",
      "accentColor": "6F5C5F"
    },
```

If you plan to display a collage of the thumbnails on a page, use the following fields:

- `thumbnailUrl` &mdash; A URL to the thumbnail of the image that `contentUrl` points to.
- `thumbnail` &mdash; The size of the thumbnail that `thumbnailUrl` points to.

For information about resizing the thumbnail, see [Resizing and cropping thumbnails](../../bing-web-search/resize-and-crop-thumbnails.md).

When the user hovers over the thumbnail, you should display the size of the original image (see the `width` and `height` fields), the encoding format (see the `encodingFormat` field), and the domain of the publisher (parse the domain from the URL in the `hostPageUrl` field).

If the user clicks on the thumbnail, you could:

- Use the URL in `webSearchUrl` to take the user to Bing where they can discover more information about the image.
- Use the token in the `imageInsightsToken` field to call [Visual Search API](../../bing-visual-search/overview.md) to display insights about the image yourself.
- Use the URL in `hostPageUrl` to take the user to the publisher's webpage where Bing found the image.

Regardless, at some level, you should give the user the option of visiting the webpage where Bing found the image.

## Getting insights about an image

Each image includes an insights token (see the `imageInsightsToken` field) that you can use to get information about the image, such as a collection of related images, web pages that include the image, or a list of merchants where you can buy the product shown in the image. For information about how to get insights, see [Visual Search API](../../bing-visual-search/overview.md).

## Adding badges to the image

Each image includes an `insightsMetadata` field, which contains a count of the number of websites where you can shop or perform other actions related to the image. For example, if the image shows an apple pie, the metadata includes a count of the number of websites where you can buy an apple pie. To indicate the number of offers in your UX, include badging such as a shopping cart icon that contains the count. When the user clicks on the icon, use the token in `imageInisghtsToken` to get the list of websites from [Visual Search API](../../bing-visual-search/overview.md).

The following list identifies the types of metadata counts that an image may include.

- `recipeSourcesCount` &mdash; The number of webpages that include recipes of the food shown in the image.
- `pagesIncludingCount` &mdash; The number of webpages that include the image.
- `availableSizesCount` &mdash; The number of different sizes (width/height) of the image that Bing found.
- `shoppingSourcesCount` &mdash; The number of websites that sell the products seen in the image.

## Using expanded queries

Most responses include the `queryExpansions` field, which contains a list of queries that expand the user’s query string in an effort to narrow the search results. This may help the user focus more on the content that they’re really interested in. For example, the query that returned the following response fragment was *black cocktail dress**. Here's the list of expanded queries that Bing returned (the bolded words are the ones that Bing added):

- Black **Lace** Cocktail Dress
- **Little** Black Cocktail Dress
- Black **Halter** Cocktail Dress

```json
  "queryExpansions": [
    {
      "text": "Black Lace Cocktail Dress",
      "displayText": "Lace",
      "webSearchUrl": "https://www.bing.com/images/search?q=Black+Lace+Cocktail...",
      "searchLink": "https://api.bing.microsoft.com/v7/images/search?q=Black+Lace+Cocktail...",
      "thumbnail": {
        "thumbnailUrl": "https://tse4.mm.bing.net/th?q=Black+Lace+Cocktail+Dress&pid=Api..."
      }
    },
    {
      "text": "Little Black Cocktail Dress",
      "displayText": "Little",
      "webSearchUrl": "https://www.bing.com/images/search?q=Little+Black+Cocktail+Dress...",
      "searchLink": "https://api.bing.microsoft.com/v7/images/search?q=Little+Black...",
      "thumbnail": {
        "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Little+Black+Cocktail+Dress&pid=Api..."
      }
    },
    {
      "text": "Black Halter Cocktail Dress",
      "displayText": "Halter",
      "webSearchUrl": "https://www.bing.com/images/search?q=Black+Halter+Cocktail+Dress...",
      "searchLink": "https://api.bing.microsoft.com/v7/images/search?q=Black+Halter...",
      "thumbnail": {
        "thumbnailUrl": "https://tse1.mm.bing.net/th?q=Black+Halter+Cocktail+Dress&pid=Api..."
      }
    },
```

The `queryExpansions` field contains a list of [Query](../reference/response-objects.md#query) objects. The `text` field contains the expanded query that you display in your UX (the `displayText` field contains the expansion term). Make `text` clickable by using the URL in `webSearchUrl` or `searchLink`. Use `webSearchUrl` to send the user to Bing's Image search results. If you provide your own results page experience, use `searchLink` to get new image search results using the expanded query string.

## Using pivot queries

If Bing can segment the user’s query, the response includes the `pivotSuggestions` field, which is a list of [Pivot](../reference/response-objects.md#pivot) objects. Each **Pivot** object contains one of the segments from the user’s query (see the `pivot` field). For example, if the user's query is *black cocktail dresses*, the pivots are *black*, *cocktail*, and *dress*. Basically, Bing replaces each pivot word in the user's query with another term. The following list contains some of the query suggestions for the *black* pivot.

- **Short** Cocktail Dresses
- **Red** Cocktail Dresses
- **Vintage** Cocktail Dresses

```json
  "pivotSuggestions": [
    {
      "pivot": "black",
      "suggestions": [
        {
          "text": "Short Cocktail Dresses",
          "displayText": "Short",
          "webSearchUrl": "https://www.bing.com/images/search?q=Short+Cocktail+Dresses...",
          "searchLink": "https://api.bing.microsoft.com/v7/images/search?q=Short+Cocktail+Dresses...",
          "thumbnail": {
            "thumbnailUrl": "https://tse4.mm.bing.net/th?q=Short+Cocktail+Dresses&pid=Ap..."
          }
        },
        {
          "text": "Red Cocktail Dresses",
          "displayText": "Red",
          "webSearchUrl": "https://www.bing.com/images/search?q=Red+Cocktail+Dresses...",
          "searchLink": "https://api.bing.microsoft.com/v7/images/search?q=Red+Cocktail+Dresses...",
          "thumbnail": {
            "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Red+Cocktail+Dresses&pid=Api..."
          }
        },

        . . .
      ]
    },
    {
      "pivot": "cocktail",
      "suggestions": [
        {
          "text": "Black Party Dresses",
          "displayText": "Party",
          "webSearchUrl": "https://www.bing.com/images/search?q=Black+Party+Dresses...",
          "searchLink": "https://api.bing.microsoft.com/v7/images/search?q=Black+Party+Dresses...",
          "thumbnail": {
            "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Black+Party+Dresses&pid=Api..."
          }
        },
        {
          "text": "Black Evening Dresses",
          "displayText": "Evening",
          "webSearchUrl": "https://www.bing.com/images/search?q=Black+Evening+Dresses...",
          "searchLink": "https://api.bing.microsoft.com/v7/images/search?q=Black+Evening+Dresses...",
          "thumbnail": {
            "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Black+Evening+Dresses&pid=Api..."
          }
        },

        . . .
      ]
    },
    {
      "pivot": "dresses",
      "suggestions": [
        {
          "text": "Black Cocktail Shoes",
          "displayText": "Shoes",
          "webSearchUrl": "https://www.bing.com/images/search?q=Black+Cocktail+Shoes...",
          "searchLink": "https://api.bing.microsoft.com/v7/images/search?q=Black+Cocktail+Shoes...",
          "thumbnail": {
            "thumbnailUrl": "https://tse4.mm.bing.net/th?q=Black+Cocktail+Shoes&pid=Api..."
          }
        },
        {
          "text": "Black Cocktail Pants",
          "displayText": "Pants",
          "webSearchUrl": "https://www.bing.com/images/search?q=Black+Cocktail+Pants...",
          "searchLink": "https://api.bing.microsoft.com/v7/images/search?q=Black+Cocktail+Pants...",
          "thumbnail": {
            "thumbnailUrl": "https://tse2.mm.bing.net/th?q=Black+Cocktail+Pants&pid=Api..."
          }
        },

        . . .
      ]
    }
  ]
```

The `pivotSuggestions` field contains the list of segments (pivots) that the original query was broken into. For each pivot, the response contains a list of [Query](../reference/response-objects.md#query) objects that contain suggested queries. The `text` field contains the suggested query. The `displayText` field contains the term that replaces the pivot in the original query.

Make `text` clickable by using the URL in `webSearchUrl` or `searchLink`. Use `webSearchUrl` to send the user to Bing's Image search results. If you provide your own results page experience, use `searchLink` to get new image search results using the pivot query string.

## Related searches answer

The `relatedSearches` field contains a list of the most popular related queries made by other users. Each [query](../reference/response-objects.md#query) in the list includes a query string (`text`), a query string with hit highlighting characters (`displayText`), and a URL (`webSearchUrl`) to Bing's Image search results page for that query.

```json
  "relatedSearches": [
    {
      "text": "Sail Dinghy",
      "displayText": "Sail Dinghy",
      "webSearchUrl": "https://www.bing.com/images/search?q=Sail+Dinghy&FORM=IRPATC",
      "searchLink": "https://api.bing.microsoft.com/v7/images/search?q=Sail+Dinghy",
      "thumbnail": {
        "thumbnailUrl": "https://tse3.mm.bing.net/th?q=Sail+Dinghy&pid=Api"
      }
    },

    . . .
  ]
```

Use the `displayText` query string and the `webSearchUrl` URL to create a hyperlink that takes the user to Bing's Image search results page for the related query. If you provide your own results page experience, use `searchLink` to get new image search results using the related query string.

For information about how to handle the highlighting markers in `displayText`, see [Hit Highlighting](../../bing-web-search/hit-highlighting.md).

## Next steps

- Learn how to [get trending images](trending-images.md).
- Learn about the [quickstarts](../quickstarts/quickstarts.md) and [samples](../samples.md) that are available to help you get up and running fast.