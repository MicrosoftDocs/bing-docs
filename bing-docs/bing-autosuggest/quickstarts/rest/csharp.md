---
title: "Quickstart: Suggest search strings with the Bing Autosuggest REST API and C#"
titleSuffix: Bing Search Services
description: Learn how to quickly start suggesting search terms with Bing Autosuggest API using C#.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-autosuggest
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Get search string suggestions using C# and Bing Autosuggest API

Use this quickstart to make your first call to Bing Autosuggest API. This C# console application sends a partial search string to Bing and parses the response. Since it's a console application, it displays a text-based version of the response for illustrative purposes only. The source code for this sample is available on <a href="https://github.com/microsoft/bing-search-dotnet-samples/blob/main/rest/quickstarts/Autosuggest.cs" target="_blank">GitHub</a>.

Grab your favorite .NET editor, JSON library, and [Create Bing Search Service resource ](../bing-web-search/create-bing-search-service-resource.md) for Bing Autosuggest and let's get started. 


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

Add a namespace and class. This example uses `AutosuggestQuickstart` for the namespace and `Program` for the class.  

```csharp
namespace AutosuggestQuickstart
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
        private static string _baseUri = "https://api.bing.microsoft.com/v7.0/suggestions";

        // The user's partial search string.

        private static string searchString = "coro";
```

Here are all the query parameters you can add to the base URI. The *q* parameter is required and you should always include the *mkt* parameter too. For information about these parameters, see [Query parameters](../../reference/query-parameters.md).

```csharp
        private const string QUERY_PARAMETER = "?q=";  // Required
        private const string MKT_PARAMETER = "&mkt=";  // Strongly suggested
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

The `RunAsync` method is where all the work happens. It builds the query string that's appended to the base URI, waits for the asynchronous HTTP request to return, deserializes the response, and either prints the suggestions or an error message.

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

                // This example uses dictionaries instead of objects to access the response data.

                var contentString = await response.Content.ReadAsStringAsync();
                Dictionary<string, object> searchResponse = JsonConvert.DeserializeObject<Dictionary<string, object>>(contentString);

                if (response.IsSuccessStatusCode)
                {
                    PrintSuggestions(searchResponse);
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
        // Makes the request to the Autosuggest endpoint.

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

That's all the more there is to sending a search request and getting back search search string suggestions. To see what the JSON response looks like, see [Handling the response](../../how-to/get-suggestions.md#handling-the-response).

The rest of the sections walk you through one way of parsing the JSON response and displaying the suggestions. Be sure to read the [use and display requirements](../../../bing-web-search/use-display-requirements.md) to make sure you comply with all display requirements.


## Displaying the suggestions

The Web suggestion group contains the list of search string suggestions. Use the search string from the `displayText` field to populate your search box's dropdown list. You can use the URL in the `url` field to take the user to Bing's search results page for the selected search string, or you can use the search string in the `query` field to call the [Web Search API](../../../bing-web-search/overview.md) yourself.

```csharp

        // Prints the list of search string suggestions from the JSON response.

        static void PrintSuggestions(Dictionary<string, object> response)
        {
            Console.WriteLine("The response contains the following search string suggestions:\n");

            var groups = response["suggestionGroups"] as Newtonsoft.Json.Linq.JToken;

            foreach (Newtonsoft.Json.Linq.JToken group in groups)
            {
                if ((string)group["name"] == "Web")
                {
                    foreach (Newtonsoft.Json.Linq.JToken suggestion in group["searchSuggestions"])
                    {
                        Console.WriteLine("Suggestion: {0} ({1})", suggestion["displayText"], suggestion["url"]);

                        // Use the suggestion in the query field if you want to call Web Search API and
                        // display the results yourself.
                    }
                }
            }
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

- For a more in depth web app example, see the [Autosuggest web app tutorial](../../tutorial/autosuggest.md).

