---
title: Bing Visual Search C# client library quickstart 
titleSuffix: Bing Search Services
description: Learn how to get image insights using the .NET client library for Bing Visual Search API.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Visual Search .NET client library

Use this quickstart to begin getting image insights from the Bing Visual Search service, using the C# client library. While Bing Visual Search has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/samples/BingSearchSamples/BingVisualSearch).

[Reference documentation](https://docs.microsoft.com/en-us/bing/search-apis/bing-visual-search/overview) | [Library source code](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/sdk/VisualSearch) | [Samples](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/samples/BingSearchSamples/BingVisualSearch)

## Prerequisites

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* If you are using Linux/MacOS, this application can be run using [Mono](https://www.mono-project.com/).
* The NuGet Visual Search package. 
    - From the Solution Explorer in Visual Studio, right-click on your project and select `Manage NuGet Packages` from the menu. Install the `Microsoft.Bing.Search.VisualSearch` package. Installing the NuGet packages also installs the following:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json

<!--
[!INCLUDE [bing-visual-search-signup-requirements](../../../../includes/bing-visual-search-signup-requirements.md)]
-->

<a name="client"></a>

## Create and initialize the application

1. In Visual Studio, create a new project. Then add the following directives.
    
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Credentials;
    using Microsoft.Bing.VisualSearch;
    using Microsoft.Bing.VisualSearch.Models;
    using Newtonsoft.Json;
    ```

2. Instantiate the client with your subscription key.
    
    ```csharp
    var client = new VisualSearchClient(new ClientCredentials(subscriptionKey));
    ```
    
## Send a search request 

1. Create a `FileStream` to your images (in this case `TestImages/image.jpg`). Then use the client to send a search request using `client.Images.VisualSearchMethodAsync()`. 
    
    ```csharp
     using (FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open))
     // The knowledgeRequest parameter is not required if an image binary is passed in the request body
     var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;
    ```
    
2. Parse the results of the previous query:

    ```csharp
    // Visual Search results
    if (visualSearchResults.Image?.ImageInsightsToken != null)
    {
        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
    }
    else
    {
        Console.WriteLine("Couldn't find image insights token!");
    }

    // List of tags
    if (visualSearchResults.Tags.Count > 0)
    {
        var firstTagResult = visualSearchResults.Tags.First();
        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");

        // List of actions in first tag
        if (firstTagResult.Actions.Count > 0)
        {
            var firstActionResult = firstTagResult.Actions.First();
            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
        }
        else
        {
            Console.WriteLine("Couldn't find tag actions!");
        }

    }
    ```

## Next steps

> [!div class="nextstepaction"]
> [Build a single-page web app](../../tutorial/visual-search-single-page-app.md)
