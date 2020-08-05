---
title: What is the Bing Image Search API?
titleSuffix: Bing Search Services
description: The Bing Image Search API enables you to use Bing's cognitive image search capabilities in your application. By sending user search queries with the API, you can get and display relevant and high-quality images similar to Bing Images.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: overview
ms.date: 07/15/2020
ms.author: scottwhi
---

# What is the Bing Image Search API?

The Bing Image Search API enables you to use Bing's image search capabilities in your application. By sending search queries to the API, you can get high-quality images similar to [bing.com/images](https://www.bing.com/images).

While the Bing Image Search API provides image-only search results, you can combine or use the other available [Bing Search APIs](../bing-web-search/bing-api-comparison.md) to find many types of content on the web.

## Bing Image Search features

| Feature                                                                                                                                                                                 | Description                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Filter and restrict image results](how-to/bing-image-search-get-images.md)                       | Filter the images that Bing returns by editing query parameters.                                                                                                       |
| [Crop, resize, and display thumbnails](../bing-web-search/resize-and-crop-thumbnails.md)                                                | Edit and display thumbnail previews for the images returned by Bing Image Search.                                                                                      |
| [Pivot & expand user search queries](how-to/bing-image-search-sending-queries.md)               | Expand your search capabilities by including and displaying Bing-suggested search terms to queries.                                                                    |
| [Get trending images](how-to/trending-images.md)                                                                     | Customize a search for trending images from around the world.                                                                                                          |

## Workflow

The Bing Image Search API is a RESTful web service, making it easy to call from any programming language that can make HTTP requests and parse JSON. You can use the service using either the [REST API](quickstarts/rest/csharp.md), or the [SDK](quickstarts/sdk/image-search-client-library-csharp.md).

1. Create a [Cognitive Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) with access to the Bing Search APIs. If you don't have an Azure subscription, you can [create an account](https://azure.microsoft.com/free/cognitive-services/) for free.
2. Send a request to the API, with a valid [search query](how-to/bing-image-search-sending-queries.md).
3. Process the API response by parsing the returned JSON message.

## Next steps

To quickly get started with your first API request, you can learn to:

* [Send search queries to Bing](quickstarts/rest/csharp.md) using the REST API, or
* [Request and filter](quickstarts/sdk/image-search-client-library-csharp.md) the images Bing returns using the SDK.

## See also

* [Pricing details](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) for the Bing Search APIs. 

* The [Bing Image Search API v7](reference/endpoints.md) reference section contains information on the API's endpoints, headers, API responses, and query parameters.

* The [Bing Use and Display Requirements](../bing-web-search/use-display-requirements.md) specify acceptable uses of the content and information gained through the Bing search APIs.

* The [Getting images from the web with the Bing Image Search API](how-to/bing-image-search-get-images.md) article describes how to search and get images from the web.

* The [Sending and working with search queries](how-to/bing-image-search-sending-queries.md) article describes how to make, customize, and pivot search queries.

