---
title: What is the Bing Video Search API?
titleSuffix: Bing Search Services
description: Learn how to search for videos across the web, using the Bing Video Search API.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-video-search
ms.topic: overview
ms.date: 07/15/2020
ms.author: scottwhi
---
# What is the Bing Video Search API?

The Bing Video Search API makes it easy to add video searching capabilities to your services and applications. By sending user search queries with the API, you can get and display relevant and high-quality videos similar to [Bing Video](https://www.bing.com/video). Use this API for search results that only contain videos. The [Bing Web Search API](../bing-web-search/overview.md) can return other types of web content, including webpages, videos, news and images.

## Bing Video Search API features

|Feature|Description
|-|-
| [Filter and restrict video results](how-to/get-videos.md#filtering-videos)                      | Filter the videos returned by editing query parameters.                                                                                                       |
| [Crop, resize, and display thumbnails](../bing-web-search/resize-and-crop-thumbnails.md)                                                | Edit and display thumbnail previews for the videos returned by Bing Video Search API.                                                                                      |
| [Get trending videos](how-to/trending-videos.md) | Search for trending videos from around the world.                                                                                                          |
| [Get video insights](how-to/video-insights.md) | Customize a search for trending videos from around the world.                                                                                                          |

## Workflow

The Bing Video Search API is a RESTful web service, making it easy to call from any programming language that can make HTTP requests and parse JSON. You can use the service using either the [REST API](quickstarts/rest/csharp.md), or the [SDK](quickstarts/sdk/video-search-client-library-csharp.md).

1. Create a [Cognitive Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) with access to the Bing Search APIs. If you don't have an Azure subscription, you can [create an account](https://azure.microsoft.com/free/cognitive-services/) for free.
2. Send a request to the API, with a valid search query.
3. Process the API response by parsing the returned JSON message.


## Next steps

Use the [quickstart](quickstarts/rest/csharp.md) to quickly get started with your first API request.

## See also

* The [Bing Video Search API v7](reference/endpoints.md) reference page contains the list of endpoints, headers, and query parameters used to request search results.

* The [Bing Use and Display Requirements](../bing-web-search/use-display-requirements.md) specify acceptable uses of the content and information gained through the Bing search APIs.
