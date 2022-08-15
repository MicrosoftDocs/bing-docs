---
title: Bing Image Search C# client library quickstart 
titleSuffix: Bing Search Services
description: The Image Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this C# quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Image Search .NET client library

Use this quickstart to make your first image search using the Bing Image Search client library, which is a wrapper for the API and contains the same features. This simple C# application sends an image search query, parses the JSON response, and displays the URL of the first image returned.

The source code for this sample is available [on GitHub](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/samples/BingSearchSamples/BingImageSearch) with additional error handling and annotations.

## Prerequisites
* Any edition of [Visual Studio 2017 or later](https://visualstudio.microsoft.com/vs/whatsnew/).


To install the Bing Image Search client library in Visual Studio, use the **Manage NuGet Packages** option from **Solution Explorer**.

<!--
[!INCLUDE [bing-image-search-signup-requirements](../../../../includes/bing-image-search-signup-requirements.md)]

-->

## Create and initialize the application

First, create a new C# console application in Visual Studio. Then add the following packages to your project.

```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Credentials;
    using Microsoft.Bing.ImageSearch;
    using Microsoft.Bing.ImageSearch.Models;
```

In the main method of your project, create variables for your valid subscription key, the image results to be returned by Bing, and a search term. Then instantiate the image search client using the key.

```csharp
//IMPORTANT: replace this variable with your Bing Apis subscription key
string subscriptionKey = "ENTER YOUR KEY HERE";
//stores the image results returned by Bing
Images imageResults = null;
// the image search term to be used in the query
string searchTerm = "canadian rockies";

//initialize the client
//NOTE: If you're using version 1.2.0 or below for the Bing Image Search client library, 
// use ImageSearchAPI() instead of ImageSearchClient() to initialize your search client.

var client = new ImageSearchClient(new ClientCredentials(subscriptionKey));
```

## Send a search query using the client

Use the client to search with a query text:

```csharp
// make the search request to the Bing Image API, and get the results"
imageResults = client.Images.SearchAsync(query: searchTerm).Result; //search query
```

## Parse and view the first image result

Parse the image results returned in the response.
If the response contains search results, store the first result and print out its details, such as a thumbnail URL, the original URL,along with the total number of returned images.  

```csharp
if (imageResults != null)
{
    //display the details for the first image result.
    var firstImageResult = imageResults.Value.First();
    Console.WriteLine($"\nTotal number of returned images: {imageResults.Value.Count}\n");
    Console.WriteLine($"Copy the following URLs to view these images on your browser.\n");
    Console.WriteLine($"URL to the first image:\n\n {firstImageResult.ContentUrl}\n");
    Console.WriteLine($"Thumbnail URL for the first image:\n\n {firstImageResult.ThumbnailUrl}");
    Console.ReadKey();
}
```

## Next steps

> [!div class="nextstepaction"]
> [Bing Image Search single-page app tutorial](../../tutorial/bing-image-search-single-page-app.md)

## See also

* [What is Bing Image Search?](../../overview.md)  
* [.NET samples for the BingApis SDK](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/samples/BingSearchSamples)
* [Bing Image Search API reference](../../reference/endpoints.md)
