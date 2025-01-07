---
title: "Quickstart: Perform a news search with C# - Bing News Search REST API"
titleSuffix: Bing Search Services
description: Use this quickstart to send a request to Bing News Search REST API using C#.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: v-apunnamara
ms.custom: seodec2018
---

# Quickstart: Search for news using C# and Bing News Search API

Use this quickstart to make your first call to Bing News Search API. This C# console application sends a search request to Bing and parses the response. Since it's a console application, it displays a text-based version of the response for illustrative purposes only. The source code for this sample is available on <a href="https://github.com/microsoft/bing-search-dotnet-samples/blob/main/rest/quickstarts/NewsSearch.cs" target="_blank">GitHub</a>. 

Grab your favorite .NET editor, JSON library, and [Create Bing Search Service resource](../../../bing-web-search/create-bing-search-service-resource.md) for Bing Image Search and let's get started. 


## Create a project and declare dependencies

Create a new project and declare the code's dependencies. This example uses <a href="https://www.newtonsoft.com/json" target="_blank">Newtonsoft</a> to parse the JSON response. Use Newtonsoft's NuGet package to install its libraries.

```csharp
using System;
using System.Net.Http.Headers;
using System.Net.Http;
using System.Threading.Tasks;
using System.Linq;
using System.Collections.Generic;
using Newtonsoft.Json;
```


## Declare a namespace and class for your program

Add a namespace and class. This example uses `NewsSearchQuickstart` for the namespace and `Program` for the class.  

```csharp
namespace NewsSearchQuickstart
{
    class Program
    {
        // The code in the following sections goes here.
    }
}
```

## Define variables

Add a few variables to the `Program` class. For simplicity, this example hardcodes the subscription key, but make sure you're pulling it from secured storage instead.

```csharp
        // In production, make sure you're pulling the subscription key from secured storage.

        private static string _subscriptionKey = "<your key goes here>"; 
        private static string _baseUri = "https://api.bing.microsoft.com/v7.0/news/search";

        // The user's search string. To get today's top stories
        // set searchString to an empty string.

        private static string searchString = "california wildfires";

        // Bing uses the X-MSEdge-ClientID header to provide users with consistent
        // behavior across Bing API calls. See the reference documentation
        // for usage.

        private static string _clientIdHeader = null;
```

Here are all the query parameters you can add to the base URI. The *q* parameter is required and you should always include the *mkt* parameter too. The rest are optional. For information about these parameters, see [Query parameters](../../reference/query-parameters.md).

```csharp
        private const string QUERY_PARAMETER = "?q=";  // Required
        private const string MKT_PARAMETER = "&mkt=";  // Strongly suggested
        private const string COUNT_PARAMETER = "&count=";
        private const string OFFSET_PARAMETER = "&offset=";
        private const string ORIGINAL_IMG_PARAMETER = "&originalImg=";
        private const string SAFE_SEARCH_PARAMETER = "&safeSearch=";
        private const string SORT_BY_PARAMETER = "&sortBy=";
        private const string TEXT_DECORATIONS_PARAMETER = "&textDecorations=";
        private const string TEXT_FORMAT_PARAMETER = "&textFormat=";
```

Use these query parameters to filter the news articles that Bing returns.

```csharp
        private const string FRESHNESS_PARAMETER = "&freshness=";
```


## Declare the Main method

Our `Main()` method is pretty simple since we're going to implement the HTTP requests asynchronously.

```csharp
        static void Main()
        {
            RunAsync().Wait();
        }
```


## Where all the work happens

The `RunAsync` method is where all the work happens. It builds the query string that's appended to the base URI, waits for the asynchronous HTTP request to return, deserializes the response, and either prints the search results or an error message.

This example uses dictionaries instead of objects to access the response data.

```csharp
        static async Task RunAsync()
        {
            try
            {
                // Remember to encode the q query parameter.

                var queryString = QUERY_PARAMETER + Uri.EscapeDataString(searchString); 
                queryString += MKT_PARAMETER + "en-us";

                HttpResponseMessage response = await MakeRequestAsync(queryString);

                _clientIdHeader = response.Headers.GetValues("X-MSEdge-ClientID").FirstOrDefault();

                // This example uses dictionaries instead of objects to access the response data.

                var contentString = await response.Content.ReadAsStringAsync();
                Dictionary<string, object> searchResponse = JsonConvert.DeserializeObject<Dictionary<string, object>>(contentString);

                if (response.IsSuccessStatusCode)
                {
                    PrintNews(searchResponse);
                }
                else
                {
                    PrintErrors(response.Headers, searchResponse);
                }
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }

            Console.WriteLine("\nPress ENTER to exit...");
            Console.ReadLine();
        }
```


## The HTTP call

Here's the HTTP request. It's your basic HTTP GET request. Use whatever HTTP client works for you.

```csharp
        // Makes the request to the News Search endpoint.

        static async Task<HttpResponseMessage> MakeRequestAsync(string queryString)
        {
            var client = new HttpClient();

            // Request headers. The subscription key is the only required header but you should
            // include User-Agent (especially for mobile), X-MSEdge-ClientID, X-Search-Location
            // and X-MSEdge-ClientIP (especially for local aware queries).

            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", _subscriptionKey);

            return (await client.GetAsync(_baseUri + queryString));
        }
```

That's all there is to sending a search request and getting back search results. To see what the answer looks like in the JSON response, see [Handling the news search response](../../how-to/search-response.md).

The rest of the sections walk you through one way of parsing the JSON response and displaying the search results. Be sure to read the [use and display requirements](../../../bing-web-search/use-display-requirements.md) to make sure you comply with all display requirements.

For information about resizing the thumbnails, see [Resizing and cropping thumbnails](../../../bing-web-search/resize-and-crop-thumbnails.md).


## Displaying the news articles

The response's `value` field contains the list of news articles. The example shows the fields you're most likely to use. For the full list of fields, see the [NewsArticle](../../reference/response-objects.md#newsarticle) object.

```csharp
        // Prints the list of news articles in the JSON response. This example prints
        // the first page of articles.

        static void PrintNews(Dictionary<string, object> response)
        {
            Newtonsoft.Json.Linq.JToken value = null;

            Console.WriteLine("The response contains the following news articles:\n");

            var articles = response["value"] as Newtonsoft.Json.Linq.JToken;

            foreach (Newtonsoft.Json.Linq.JToken article in articles)
            {
                Console.WriteLine("Title: " + article["name"]);
                Console.WriteLine("URL to article: " + article["url"]);
                Console.WriteLine("Description: " + article["description"]);
                Console.WriteLine("Publisher: " + GetPublisherString(article["provider"]));

                if ((value = article["image"]) != null)
                {
                    Console.WriteLine("Thumbnail: " + value["thumbnail"]["contentUrl"]);
                    Console.WriteLine("Thumbnail size: {0} (w) x {1} (h) ", value["thumbnail"]["width"], value["thumbnail"]["height"]);
                }

                if ((value = article["video"]) != null)
                {
                    if (value["motionThumbnailUrl"] != null)
                    {
                        Console.WriteLine("Title: " + value["name"]);
                        Console.WriteLine("Motion thumbnail: " + value["motionThumbnailUrl"]);
                        Console.WriteLine("Motion thumbnail size: {0} (w) x {1} (h) ", value["thumbnail"]["width"], value["thumbnail"]["height"]);
                    }
                }

                Console.WriteLine();
            }
        }

        // Get a printable publisher string. The article's publisher field is an array
        // of publishers. In practice, there's a single publisher, but...

        static string GetPublisherString(Newtonsoft.Json.Linq.JToken publishers)
        {
            string publisherString = "";
            Boolean isFirst = true;

            foreach (Newtonsoft.Json.Linq.JToken publisher in publishers)
            {
                if (!isFirst)
                {
                    publisherString += " | ";
                }

                publisherString += publisher["name"];
            }

            return publisherString;
        }
```


## Handling errors

This section shows an option for handling errors that the service may return. For example, the service returns an error if your subscription key is not valid or is not valid for the specified endpoint. The service may also return an error if you specify a parameter value that's not valid.

```csharp
        // Print any errors that occur. Depending on which part of the service is
        // throwing the error, the response may contain different error formats.

        static void PrintErrors(HttpResponseHeaders headers, Dictionary<String, object> response)
        {
            Console.WriteLine("The response contains the following errors:\n");

            object value;

            if (response.TryGetValue("error", out value))  // typically 401, 403
            {
                PrintError(response["error"] as Newtonsoft.Json.Linq.JToken);
            }
            else if (response.TryGetValue("errors", out value))
            {
                // Bing API error

                foreach (Newtonsoft.Json.Linq.JToken error in response["errors"] as Newtonsoft.Json.Linq.JToken)
                {
                    PrintError(error);
                }

                // Included only when HTTP status code is 400; not included with 401 or 403.

                IEnumerable<string> headerValues;
                if (headers.TryGetValues("BingAPIs-TraceId", out headerValues))
                {
                    Console.WriteLine("\nTrace ID: " + headerValues.FirstOrDefault());
                }
            }

        }

        static void PrintError(Newtonsoft.Json.Linq.JToken error)
        {
            string value = null;

            Console.WriteLine("Code: " + error["code"]);
            Console.WriteLine("Message: " + error["message"]);

            if ((value = (string)error["parameter"]) != null)
            {
                    Console.WriteLine("Parameter: " + value);
            }

            if ((value = (string)error["value"]) != null)
            {
                Console.WriteLine("Value: " + value);
            }
        }
```


## Next steps

- For a more in depth web app example, see the [News web app tutorial](../../tutorial/bing-news-search-single-page-app.md).

