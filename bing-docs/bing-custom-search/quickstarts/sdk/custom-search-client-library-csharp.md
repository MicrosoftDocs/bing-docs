---
title: Bing Custom Search C# client library quickstart 
titleSuffix: Bing Search Services
description: The Custom Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this C# quickstart to send search requests and get back results from your instance.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Custom Search .NET client library

Get started with the Bing Custom Search client library for C#. Follow these steps to install the package and try out the example code for basic tasks. The Bing Custom Search API enables you to create tailored, ad-free search experiences for topics that you care about. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/samples/BingSearchSamples/BingCustomWebSearch).

Use the Bing Custom Search client library for C# to:
* Find search results on the web, from your Bing Custom Search instance.

[Reference documentation](https://www.customsearch.ai/) | [Library source code](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/sdk/CustomWebSearch) | [Samples](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/samples/BingSearchSamples/BingCustomWebSearch)


## Prerequisites

- A Bing Custom Search instance. See [Quickstart: Create your first Bing Custom Search instance](../../how-to/quick-start.md) for more information.
- Microsoft [.NET Core](https://www.microsoft.com/net/download/core)
- Any edition of [Visual Studio 2017 or later](https://www.visualstudio.com/downloads/)
- If you are using Linux/MacOS, this application can be run using [Mono](https://www.mono-project.com/).
 
- From **Solution Explorer** in Visual Studio, right-click your project and select **Manage NuGet Packages** from the menu. Install the `Microsoft.Bing.Search.CustomSearch` package. Installing the NuGet Custom Search package also installs the following assemblies:
    - Microsoft.Rest.ClientRuntime
    - Microsoft.Rest.ClientRuntime.Azure
    - Newtonsoft.Json

<!--
[!INCLUDE [bing-custom-search-prerequisites](../../../../includes/bing-custom-search-signup-requirements.md)]
-->

## Create and initialize the application

1. Create a new C# console application in Visual Studio. Then add the following packages to your project.

    ```csharp
    using System;
    using System.Linq;
    using System.Text;
    using Microsoft.Bing.CustomSearch;
    using Microsoft.Bing.CustomSearch.Models;
    using Newtonsoft.Json;
    using System.Collections.Generic;
    using Microsoft.Rest;
    using Microsoft.Rest.Serialization;
    using System.Collections;
    using System.Net;
    using System.Net.Http;
    using System.Threading;
    using System.Threading.Tasks;
    using Credentials;
    ```

2. In the main method of your application, instantiate the search client with your API key.

    ```csharp
    var client = new CustomSearchClient(new ClientCredentials(subscriptionKey));
    ```

## Send the search request and receive a response
    
1. Send a search query using your client's `SearchAsync()` method, and save the response. Be sure to replace your `YOUR-CUSTOM-CONFIG-ID` with your instance's configuration ID (you can find the ID in the [Bing Custom Search portal](https://www.customsearch.ai/)). This example searches for "Xbox".

    ```csharp
    // This will look up a single query (Xbox).
    var webData = client.CustomInstance.SearchWithHttpMessagesAsync(query: "Xbox", customConfig: YOUR-CUSTOM-CONFIG-ID).Result;
    ```

2. The `SearchAsync()` method returns a `WebData` object. Use the object to iterate through any `WebPages` that were found. This code finds the first webpage result and prints the webpage's `Name` and `URL`.

    ```csharp
      //WebPages
        if (webData?.Body.WebPages?.Value?.Count > 0)
        {
            // find the first web page
            var firstWebPagesResult = webData.Body.WebPages.Value.FirstOrDefault();

            if (firstWebPagesResult != null)
            {
                Console.WriteLine("Webpage Results#{0}", webData.Body.WebPages.Value.Count);
                Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
                Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
            }
            else
            {
                Console.WriteLine("Couldn't find web results!");
            }
        }
        else
        {
            Console.WriteLine("Didn't see any Web data..");
        }
    ```

## Next steps

> [!div class="nextstepaction"]
> [Build a Custom Search web app](../../tutorial/custom-search-web-page.md)
