---
title: Bing Video Search C# client library quickstart 
titleSuffix: Bing Search Services
description: The Video Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this C# quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Video Search .NET client library

Use this quickstart to begin searching for news with the Bing Video Search client library for C#. While Bing Video Search has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/samples/BingSearchSamples/BingVideoSearch) with additional annotations, and features.

## Prerequisites

* Any edition of [Visual Studio 2017 or later](https://visualstudio.microsoft.com/downloads/).
* The Json.NET framework, available [as a NuGet package](https://www.nuget.org/packages/Newtonsoft.Json/).

To add the Bing Video Search client library to your project, select **Manage NuGet Packages** from **Solution Explorer** in Visual Studio. Add the `Microsoft.Bing.Search.VideoSearch` package.

Installing the dependencies:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

<!--
[!INCLUDE [bing-video-search-signup-requirements](../../../../includes/bing-video-search-signup-requirements.md)]
-->

## Create and initialize a project

1. Create a new C# console solution in Visual Studio. Then add the following into the main code file.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Credentials;
    using Microsoft.Bing.VideoSearch;
    using Microsoft.Bing.VideoSearch.Models;
    ```

2. Instantiate the client by creating a new `ClientCredentials` object with your subscription key, and calling the constructor.

    ```csharp
    var client = new VideoSearchClient(new ClientCredentials(subscriptionKey));
    ```

## Send a search request and process the results

1. Use the client to send a search request. Use "Nasa CubeSat" for the search query.

    ```csharp
    var videoResults = client.Videos.SearchAsync(query: "Nasa CubeSat").Result;
    ```

2. If any results were returned, get the first one with `videoResults.Value[0]`. Then print the video's ID, title, and url.

    ```csharp
    if (videoResults.Value.Count > 0)
    {
        var firstVideoResult = videoResults.Value[0];

        Console.WriteLine($"\r\nVideo result count: {videoResults.Value.Count}");
        Console.WriteLine($"First video id: {firstVideoResult.VideoId}");
        Console.WriteLine($"First video name: {firstVideoResult.Name}");
        Console.WriteLine($"First video url: {firstVideoResult.ContentUrl}");
    }
    else
    {
        Console.WriteLine("Couldn't find video results!");
    }
    ```

## Next steps

> [!div class="nextstepaction"]
> [Create a single page web app](../../tutorial/bing-video-search-single-page-app.md)

## See also 

* [What is the Bing Video Search API?](../../overview.md)
* [BingApis .NET SDK samples](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/samples/BingSearchSamples)
