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

# Quickstart: Search your custom view of the web using C# and Bing Custom Search API

Use this quickstart to make your first call to Bing Custom Search API. This C# console application sends a search request to Bing and parses the response. Since it's a console application, it displays a text-based version of the response for illustrative purposes only.  The source code for this sample is available on <a href="https://github.com/microsoft/bing-search-dotnet-samples/blob/main/rest/quickstarts/CustomSearch.cs" target="_blank">GitHub</a>.

If you've created your Custom Search instance (view), grab your favorite .NET editor, JSON library, and [Create Bing Search Service resource](../../bing-web-search/create-bing-search-service-resource.md) for Bing Custom Search and let's get started. Otherwise, please [create your instance](../../how-to/define-your-custom-view.md).


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

Add a namespace and class. This example uses `CustomWebSearchQuickstart` for the namespace and `Program` for the class.  

```csharp
namespace CustomWebSearchQuickstart
{
    class Program
    {
        // The code in the following sections goes here.
    }
}
```

## Define variables

Add a few variables to the `Program` class. For simplicity, this example hardcodes the subscription key and configuration ID, but make sure you're pulling them from secured storage instead.

```csharp
        // In production, make sure you're pulling the subscription key and configuration
        // ID from secured storage.

        private static string _subscriptionKey = "<your key goes here>"; 
        private static string _baseUri = "https://api.bing.microsoft.com/v7.0/custom/search";
        private static string _customConfigId = "<your custom instance configuration ID goes here>";

        // The user's search string.

        private static string searchString = "surface book 3";

        // Bing uses the X-MSEdge-ClientID header to provide users with consistent
        // behavior across Bing API calls. See the reference documentation
        // for usage.

        private static string _clientIdHeader = null;
```

Here are all the query parameters you can add to the base URI. The *q* parameter is required and you should always include the *mkt* parameter too. The rest are optional. For information about these parameters, see [Query parameters](../../reference/query-parameters.md).

```csharp
        private const string QUERY_PARAMETER = "?q=";  // Required
        private const string CUSTOM_CONFIG_PARAMETER = "&customConfig=";  // Required
        private const string MKT_PARAMETER = "&mkt=";  // Strongly suggested
        private const string COUNT_PARAMETER = "&count=";
        private const string OFFSET_PARAMETER = "&offset=";
        private const string SAFE_SEARCH_PARAMETER = "&safeSearch=";
        private const string TEXT_DECORATIONS_PARAMETER = "&textDecorations=";
        private const string TEXT_FORMAT_PARAMETER = "&textFormat=";
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
                // Remember to encode query parameters like q, responseFilters, promote, etc.

                var queryString = QUERY_PARAMETER + Uri.EscapeDataString(searchString);
                queryString += CUSTOM_CONFIG_PARAMETER + _customConfigId;
                queryString += MKT_PARAMETER + "en-us";
                queryString += TEXT_DECORATIONS_PARAMETER + Boolean.TrueString;

                HttpResponseMessage response = await MakeRequestAsync(queryString);

                _clientIdHeader = response.Headers.GetValues("X-MSEdge-ClientID").FirstOrDefault();

                // This example uses dictionaries instead of objects to access the response data.

                var contentString = await response.Content.ReadAsStringAsync();
                Dictionary<string, object> searchResponse = JsonConvert.DeserializeObject<Dictionary<string, object>>(contentString);

                if (response.IsSuccessStatusCode)
                {
                    PrintResponse(searchResponse);
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
        // Makes the request to the Web Search endpoint.

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

That's all the more there is to sending a search request and getting back search results. The rest of the sections walk you through one way of parsing the JSON response and displaying the search results. Be sure to read the [use and display requirements](../../../bing-web-search/use-display-requirements.md) to make sure you comply with all display requirements.


## Using ranking to display the search results

If the request succeeds, the code calls the `PrintResponse` method to print the search results in the console window.

The example uses the RankingResponse answer to display the search results. The ranking determines which answers to display in the pole, mainline, and sidebar sections of the search results page. For information about using the RankingResponse answer, see [Use ranking to display search results](../../../bing-web-search/rank-results.md). 

```csharp
        // Prints the JSON response data for pole, mainline, and sidebar.

        static void PrintResponse(Dictionary<string, object> response)
        {
            Console.WriteLine("The response contains the following answers:\n");

            var ranking = response["rankingResponse"] as Newtonsoft.Json.Linq.JToken; 

            Newtonsoft.Json.Linq.JToken position;

            if ((position = ranking["pole"]) != null)
            {
                Console.WriteLine("Pole Position:\n");
                DisplayAnswersByRank(position["items"], response);
            }

            if ((position = ranking["mainline"]) != null)
            {
                Console.WriteLine("Mainline Position:\n");
                DisplayAnswersByRank(position["items"], response);
            }

            if ((position = ranking["sidebar"]) != null)
            {
                Console.WriteLine("Sidebar Position:\n");
                DisplayAnswersByRank(position["items"], response);
            }
        }
```

### Display all results for an answer or a single result

Each item in the ranking tells you whether to display all results from the answer together or to display a single result from the answer. If the item includes the `resultIndex` field, use the index value to display that single result from the answer. But if the item doesn't include `resultIndex`, you display all results from the answer. 

```csharp
        // Displays each result based on ranking. Ranking contains the results for
        // the pole, mainline, or sidebar section of the search results.

        static void DisplayAnswersByRank(Newtonsoft.Json.Linq.JToken items, Dictionary<string, object> response)
        {
            foreach (Newtonsoft.Json.Linq.JToken item in items)
            {
                var answerType = (string)item["answerType"];
                Newtonsoft.Json.Linq.JToken index = -1;

                // If the ranking item doesn't include an index of the result to  
                // display, then display all the results for that answer.

                if ("WebPages" == answerType)
                {
                    if ((index = item["resultIndex"]) == null)
                    {
                        DisplayAllWebPages(((Newtonsoft.Json.Linq.JToken)response["webPages"])["value"]);
                    }
                    else
                    {
                        DisplayWegPage(((Newtonsoft.Json.Linq.JToken)response["webPages"])["value"].ElementAt((int)index));
                    }
                }
                else
                {
                    Console.WriteLine("\nUnknown answer type: {0}\n", answerType);
                }
            }
        }
```

### Display answer results

This example accesses a few of the fields from each type of answer result and applies any contractual rules. You will likely use data from more of the fields than are shown in this example. For information about the fields that each answer result may include, see [Response objects](../../reference/response-objects.md). 

```csharp
        // Displays all webpages in the Webpages answer.
        static void DisplayAllWebPages(Newtonsoft.Json.Linq.JToken webpages)
        {
            foreach (Newtonsoft.Json.Linq.JToken webpage in webpages)
            {
                DisplayWegPage(webpage);
            }
        }

        // Displays a single webpage.
        static void DisplayWegPage(Newtonsoft.Json.Linq.JToken webpage)
        {
            // Some webpages require attribution. Checks if this page requires
            // attribution and gets the list of attributions to apply.

            Dictionary<string, string> rulesByField = null;
            rulesByField = GetRulesByField(webpage["contractualRules"]);

            Console.WriteLine("\tWebpage\n");
            Console.WriteLine("\t\tName: " + webpage["name"]);
            Console.WriteLine("\t\tUrl: " + webpage["url"]);
            Console.WriteLine("\t\tDisplayUrl: " + webpage["displayUrl"]);
            Console.WriteLine("\t\tSnippet: " + webpage["snippet"]);

            // Apply attributions if they exist.

            if (null != rulesByField && null != rulesByField["snippet"])
            {
                Console.WriteLine("\t\t\tData from: " + rulesByField["snippet"]);
            }

            Console.WriteLine();

        }
```

### Handling contractual rules

You need to check each result to see if it includes one or more contractual rules. Some webpages contain contractual rules. Some rules apply to the result as a whole and others apply to a specific field. If the rule applies to a specific field, it includes the `targetPropertyName` field, which contains the name of the target field. 

This example, builds a dictionary of the rules that the calling method accesses when it displays the result. If the rule applies to the result as a whole, the key `global`. Otherwise, the key is the name of the field that the rule targets.

```csharp
        // Checks if the result includes contractual rules and builds a dictionary of 
        // the rules. 

        static Dictionary<string, string> GetRulesByField(Newtonsoft.Json.Linq.JToken contractualRules)
        {
            if (null == contractualRules)
            {
                return null;
            }

            var rules = new Dictionary<string, string>();

            foreach (Newtonsoft.Json.Linq.JToken rule in contractualRules as Newtonsoft.Json.Linq.JToken)
            {
                // Use the rule's type as the key.

                string key = null;
                string value = null;
                var index = ((string)rule["_type"]).LastIndexOf('/');
                var ruleType = ((string)rule["_type"]).Substring(index + 1);
                string attribution = null;

                if (ruleType == "LicenseAttribution")
                {
                    attribution = (string)rule["licenseNotice"];
                }
                else if (ruleType == "LinkAttribution")
                {
                    attribution = string.Format("{0}({1})", (string)rule["text"], (string)rule["url"]);
                }
                else if (ruleType == "MediaAttribution")
                {
                    attribution = (string)rule["url"];
                }
                else if (ruleType == "TextAttribution")
                {
                    attribution = (string)rule["text"];
                }

                // If the rule targets specific data in the result; for example, the
                // snippet field, use the target's name as the key. Multiple rules
                // can apply to the same field. 

                if ((key = (string) rule["targetPropertyName"]) != null)
                {
                    if (rules.TryGetValue(key, out value))
                    {
                        rules[key] = value + " | " + attribution;
                    }
                    else
                    {
                        rules.Add(key, attribution);
                    }
                }
                else
                {
                    // Otherwise, the rule applies to the result. Uses 'global' as the key
                    // value for this case.

                    key = "global";

                    if (rules.TryGetValue(key, out value))
                    {
                        rules[key] = value + " | " + attribution;
                    }
                    else
                    {
                        rules.Add(key, attribution);
                    }
                }
            }

            return rules;
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

- For a more in depth web app example, see the [Custom Web Search tutorial](../../tutorial/custom-search-web-page.md).

