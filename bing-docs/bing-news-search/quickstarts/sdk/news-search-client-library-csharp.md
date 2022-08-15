---
title: Bing News Search C# client library quickstart 
titleSuffix: Bing Search Services
description: The News Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this C# quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing News Search .NET client library

Use this quickstart to begin searching for news with the Bing News Search client library for C#. While Bing News Search has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/samples/BingSearchSamples/BingNewsSearch).

## Prerequisites

* Any edition of [Visual Studio 2017 or later](https://www.visualstudio.com/downloads/).
* The [Json.NET](https://www.newtonsoft.com/json) framework, available as a NuGet package.
* If you are using Linux/MacOS, this application can be run using [Mono](https://www.mono-project.com/).

* install the following:
    * Microsoft.Rest.ClientRuntime
    * Microsoft.Rest.ClientRuntime.Azure
    * Newtonsoft.Json

To set up a console application using the Bing News Search client library, browse to the `Manage NuGet Packages` option from the Solution Explorer in Visual Studio.  Add the `Microsoft.Bing.Search.NewsSearch` package.

<!--
[!INCLUDE [bing-news-search-signup-requirements](../../../../includes/bing-news-search-signup-requirements.md)]
-->

## Create and initialize a project

1. Create a new C# console solution in Visual Studio. Then add the following into the main code file.
    
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Credentials;
    using Microsoft.microsoft.Bing.NewsSearch;
    using Microsoft.microsoft.Bing.NewsSearch.Models;
    ```

2. Create a variable for your API key, a search term, and then instantiate the news search client with it.

    ```csharp
    var client = new NewsSearchClient(new ClientCredentials(subscriptionKey));
    ```

## Send a request, and parse the result

1. Use the client to send a search request to the Bing News Search service:
    ```csharp
    var newsResults = client.News.SearchAsync(query: "Quantum  Computing", market: "en-us", count: 10).Result;
    ```

2. If any results were returned, parse them:

    ```csharp
    if (newsResults.Value.Count > 0)
    {
        var firstNewsResult = newsResults.Value[0];
    
        Console.WriteLine($"TotalEstimatedMatches value: {newsResults.TotalEstimatedMatches}");
        Console.WriteLine($"News result count: {newsResults.Value.Count}");
        Console.WriteLine($"First news name: {firstNewsResult.Name}");
        Console.WriteLine($"First news url: {firstNewsResult.Url}");
        Console.WriteLine($"First news description: {firstNewsResult.Description}");
        Console.WriteLine($"First news published time: {firstNewsResult.DatePublished}");
        Console.WriteLine($"First news provider: {firstNewsResult.Provider[0].Name}");
    }
    
    else
    {
        Console.WriteLine("Couldn't find news results!");
    }
    Console.WriteLine("Enter any key to exit...");
    Console.ReadKey();
    ```

## Next steps

> [!div class="nextstepaction"]
> [Create a single-page web app](../../tutorial/bing-news-search-single-page-app.md)
