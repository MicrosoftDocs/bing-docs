---
title: "Quickstart: Call your Bing Custom Search endpoint using C# | Microsoft Docs"
titleSuffix: Bing Search Services
description: Use this quickstart to begin requesting search results from your Bing Custom Search instance in C#. 
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Call your Bing Custom Search endpoint using C# 

Use this quickstart to learn how to request search results from your Bing Custom Search instance. Although this application is written in C#, the Bing Custom Search API is a RESTful web service compatible with most programming languages. The source code for this sample is available on [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingCustomSearchv7.cs).

## Prerequisites

- A Bing Custom Search instance. For more information, see [Quickstart: Create your first Bing Custom Search instance](../../how-to/quick-start.md).
- [Microsoft .NET Core](https://www.microsoft.com/net/download/core).
- Any edition of [Visual Studio 2019 or later](https://www.visualstudio.com/downloads/).
- If you're using Linux/MacOS, this application can be run using [Mono](https://www.mono-project.com/).
- The [Bing Custom Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/2.0.0) NuGet package. 

   To install this package in Visual Studio: 
     1. Right-click your project in **Solution Explorer**, and then select **Manage NuGet Packages**. 
     2. Search for and select *Microsoft.Azure.CognitiveServices.Search.CustomSearch*, and then install the package.

   When you install the Bing Custom Search NuGet package, Visual Studio also installs the following packages:
     - **Microsoft.Rest.ClientRuntime**
     - **Microsoft.Rest.ClientRuntime.Azure**
     - **Newtonsoft.Json**

<!--
[!INCLUDE [bing-custom-search-prerequisites](../../../../includes/bing-custom-search-signup-requirements.md)]
-->

## Create and initialize the application

1. Create a new C# console application in Visual Studio. Then, add the following packages to your project:

    ```csharp
    using System;
    using System.Net.Http;
    using System.Web;
    using Newtonsoft.Json;
    ```

2. Create the following classes to store the search results returned by the Bing Custom Search API:

    ```csharp
    public class BingCustomSearchResponse {        
        public string _type{ get; set; }            
        public WebPages webPages { get; set; }
    }

    public class WebPages {
        public string webSearchUrl { get; set; }
        public int totalEstimatedMatches { get; set; }
        public WebPage[] value { get; set; }        
    }

    public class WebPage {
        public string name { get; set; }
        public string url { get; set; }
        public string displayUrl { get; set; }
        public string snippet { get; set; }
        public DateTime dateLastCrawled { get; set; }
        public string cachedPageUrl { get; set; }
    }
    ```

3. In the main method of your project, create the following variables for your Bing Custom Search API subscription key, search instance's custom configuration ID, and search term:

    ```csharp
    var subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    var customConfigId = "YOUR-CUSTOM-CONFIG-ID";
    var searchTerm = args.Length > 0 ? args[0]:"microsoft";
    ```

4. Construct the request URL by appending your search term to the `q=` query parameter, and your search instance's custom configuration ID to the `customconfig=` parameter. Separate the parameters with an ampersand (`&`). 

    ```csharp
    var url = "https://api.bing.microsoft.com/bingcustomsearch/v7.0/search?" +
                "q=" + searchTerm + "&" +
                "customconfig=" + customConfigId;
    ```

## Send and receive a search request 

1. Create a request client, and add your subscription key to the `Ocp-Apim-Subscription-Key` header.

    ```csharp
    var client = new HttpClient();
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    ```

2. Perform the search request and get the response as a JSON object.

    ```csharp
    var httpResponseMessage = client.GetAsync(url).Result;
    var responseContent = httpResponseMessage.Content.ReadAsStringAsync().Result;
    BingCustomSearchResponse response = JsonConvert.DeserializeObject<BingCustomSearchResponse>(responseContent);
    ```
## Process and view the results

- Iterate over the response object to display information about each search result, including its name, url, and the date the webpage was last crawled.

    ```csharp
    for(int i = 0; i < response.webPages.value.Length; i++) {                
        var webPage = response.webPages.value[i];
        
        Console.WriteLine("name: " + webPage.name);
        Console.WriteLine("url: " + webPage.url);                
        Console.WriteLine("displayUrl: " + webPage.displayUrl);
        Console.WriteLine("snippet: " + webPage.snippet);
        Console.WriteLine("dateLastCrawled: " + webPage.dateLastCrawled);
        Console.WriteLine();
    }
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();
    ```

## Next steps

> [!div class="nextstepaction"]
> [Build a Custom Search web app](../../tutorial/custom-search-web-page.md)
