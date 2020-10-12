---
title: "Quickstart: Check spelling with the REST API and C# - Bing Spell Check"
titleSuffix: Bing Search Services
description: Get started with Bing Spell Check REST API to check spelling and grammar using C#.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Check spelling using C# and Bing Spell Check API

Use this quickstart to make your first call to Bing Spell Check API. This C# console application sends Bing a text string to check for spelling and grammar issues and parses the response. Since it's a console application, it displays a text-based version of the response for illustrative purposes only. The source code for this sample is available on <a href="https://github.com/microsoft/bing-search-dotnet-samples/blob/main/rest/quickstarts/SpellCheck.cs" target="_blank">GitHub</a>. 

Grab your favorite .NET editor, JSON library, and [subscription key](../../../bing-web-search/get-subscription-key.md) for Bing Spell Check and let's get started. 


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

Add a namespace and class. This example uses `SpellCheckQuickstart` for the namespace and `Program` for the class.  

```csharp
namespace SpellCheckQuickstart
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
        private static string _baseUri = "https://api.bing.microsoft.com/v7.0/spellcheck";

        // The text to spell check.

        private static string _spellCheckString = "when its your turn turn, john, come runing";
```

Here are all the query parameters you're likely to use. The *text* parameter is required and you should always include the *mkt* parameter too. For information about these and other parameters you may specify, see [Query parameters](../../reference/query-parameters.md).

```csharp
        // The query parameters you're most likely to use.

        private const string TEXT_PARAMETER = "?text=";  // Required
        private const string MKT_PARAMETER = "&mkt=";    // Strongly suggested
        private const string MODE_PARAMETER = "&mode=";  // proof (default), spell
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

The `RunAsync` method is where all the work happens. It builds the query string that's appended to the base URI, waits for the asynchronous HTTP request to return, deserializes the response, and either prints the spelling and grammar results or an error message.

This example uses dictionaries instead of objects to access the response data.

```csharp
        static async Task RunAsync()
        {
            try
            {
                // Remember to encode the text query parameter.

                var queryString = TEXT_PARAMETER + Uri.EscapeDataString(_spellCheckString);
                queryString += MODE_PARAMETER + "proof"; // "spell";
                queryString += MKT_PARAMETER + "en-us";

                HttpResponseMessage response = await MakeRequestAsync(queryString);

                _clientIdHeader = response.Headers.GetValues("X-MSEdge-ClientID").FirstOrDefault();

                // This example uses dictionaries instead of objects to access the response data.

                var contentString = await response.Content.ReadAsStringAsync();
                Dictionary<string, object> searchResponse = JsonConvert.DeserializeObject<Dictionary<string, object>>(contentString);

                if (response.IsSuccessStatusCode)
                {
                    PrintSpellCheckResults(searchResponse);
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
        // Makes the request to the Spell Check endpoint.

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

That's all the more there is to sending spell check request and getting back results. To see what the answer looks like in the JSON response, see [Handling the search response](../../how-to/search-response.md).

The rest of the sections walk you through one way of parsing the JSON response and displaying the results. Be sure to read the [use and display requirements](../../../bing-web-search/use-display-requirements.md) to make sure you comply with all display requirements.


## Displaying the spelling results

The response's `flaggedTokens` field contains the list of spelling and grammar issues. The example shows the fields you're most likely to use. For the full list of fields, see the [FlaggedToken](../../reference/response-objects.md#flaggedtoken) object.

```csharp
        // Prints the list of spelling issues and the corrected string.

        static void PrintSpellCheckResults(Dictionary<string, object> response)
        {
            string updatedTextString = _spellCheckString;
            int adjustOffset = 0;
            int tokenLength = 0;
            bool isRemoveTokenCase = false;

            Console.WriteLine("The response contains the following spelling issues:\n");

            var tokens = response["flaggedTokens"] as Newtonsoft.Json.Linq.JToken;

            foreach (Newtonsoft.Json.Linq.JToken token in tokens)
            {
                Console.WriteLine("Word: " + token["token"]);
                Console.WriteLine("Suggestion: " + token["suggestions"][0]["suggestion"]);
                Console.WriteLine("Offset: " + token["offset"]);
                Console.WriteLine();

                tokenLength = ((string)token["token"]).Length;

                // Repeat token case

                if ((string)(token["suggestions"][0]["suggestion"]) == string.Empty)
                {
                    isRemoveTokenCase = true;
                    adjustOffset--;
                    tokenLength++;
                }

                updatedTextString = updatedTextString.Remove((int)token["offset"] + adjustOffset, tokenLength);

                if (!isRemoveTokenCase)
                {
                    updatedTextString = updatedTextString.Insert((int)token["offset"] + adjustOffset, (string)token["suggestions"][0]["suggestion"]);
                }
                else
                {
                    isRemoveTokenCase = false;
                    adjustOffset++;
                }

                // The token offset value is the offset into the original string but
                // we need the offset into the updated text string after applying the
                // changes.

                adjustOffset += ((string)token["suggestions"][0]["suggestion"]).Length - tokenLength;
            }

            Console.WriteLine("Original text: " + _spellCheckString);
            Console.WriteLine("Updated text: " + updatedTextString);
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


## Using a POST request

You can send requests using an HTTP GET or POST request. Use GET requests if your strings are always less than 1,500 characters; otherwise, for longer text or for documentation scenarios, use POST. Make the following changes to the quickstart to use POST.

Replace the `TEXT_PARAMETER` constant with:

```csharp
        private const string TEXT_PARAMETER = "text=";  // Required
```

And replace the `MakeRequestAsync` method with:

```csharp
        // Makes the request to the Spell Check endpoint.

        static async Task<HttpResponseMessage> MakeRequestAsync(string queryString)
        {
            var client = new HttpClient();

            // Request headers. The subscription key is the only required header but you should
            // include User-Agent (especially for mobile), X-MSEdge-ClientID, X-Search-Location
            // and X-MSEdge-ClientIP (especially for local aware queries).

            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", _subscriptionKey);

            var postContent = new StringContent(queryString, System.Text.Encoding.UTF8, "application/x-www-form-urlencoded");

            return (await client.PostAsync(_baseUri, postContent));
        }
```


## Next steps

- For a more in depth web app example, see the [Spell check web app tutorial](../../tutorial/spellcheck.md).

