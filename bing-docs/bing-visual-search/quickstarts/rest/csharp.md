---
title: "Quickstart: Get image insights using the REST API and C# - Bing Visual Search"
titleSuffix: Bing Search Services
description: Learn how to get image insights using C# and Bing Visual Search API.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Get image insights using C# and Bing Visual Search API

Use this quickstart to make your first call to Bing Visual Search API. This C# console application sends a request to Bing and parses the response. Since it's a console application, it displays a text-based version of the response for illustrative purposes only. The source code for this sample is available on <a href="https://github.com/microsoft/bing-search-dotnet-samples/blob/main/rest/quickstarts/VisualSearch.cs" target="_blank">GitHub</a>. 

Grab your favorite .NET editor, JSON library, and [subscription key](../../../bing-web-search/get-subscription-key.md) for Bing Visual Search and let's get started. 


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

Add a namespace and class. This example uses `VisualSearchQuickstart` for the namespace and `Program` for the class.  

```csharp
namespace VisualSearchQuickstart
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
        private static string _baseUri = "https://api.bing.microsoft.com/v7.0/images/visualsearch";

        // To page through visually similar images or visually similar products, 
        // you'll need the next offset that Bing returns.

        private static long _nextOffset = 0;

        // To get additional insights about the image, you'll need the image's
        // insights token (see Visual Search API), URL, or binary.

        private static string _insightsToken = null;
        private static string _imageUrl = null;

        // Bing uses the X-MSEdge-ClientID header to provide users with consistent
        // behavior across Bing API calls. See the reference documentation
        // for usage.

        private static string _clientIdHeader = null;
```

Here are all the query parameters you can add to the base URI. For information about these parameters, see [Query parameters](../../reference/query-parameters.md).

```csharp
        private const string MKT_PARAMETER = "?mkt=";  // Strongly suggested
        private const string SAFE_SEARCH_PARAMETER = "&safeSearch=";
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

The `RunAsync` method is where all the work happens. It builds the query string that's appended to the base URI, builds the form data that goes in the body of the POST, waits for the asynchronous HTTP request to return, deserializes the response, and either prints the insights or an error message.

This example uses dictionaries instead of objects to access the response data.

```csharp
        static async Task RunAsync()
        {
            try
            {
                var queryString = MKT_PARAMETER + "en-us";

                // Build the form data.

                using (var postContent = new MultipartFormDataContent("boundary_" + DateTime.Now.ToString(CultureInfo.InvariantCulture)))
                {
                    // Builds the body of the request. This example shows
                    // just some of the fields you can specify.

                    var visualSearchParams = new Dictionary<string, object>()
                    {
                        {"ImageInfo", new Dictionary<string, object>()
                            {
                                {"ImageInsightsToken", _insightsToken}
                            }
                        },
                        {"KnowledgeRequest", new Dictionary<string, object>()
                            {
                                {"InvokedSkillsRequestData",  new Dictionary<string, object>()
                                    {
                                        {"EnableEntityData", true}
                                    }
                                }
                            }
                        }
                    };

                    // Searializes the parameters object into a string and 
                    // adds it to the form data.

                    using (var jsonContent = new StringContent(JsonConvert.SerializeObject(visualSearchParams,
                        new JsonSerializerSettings() { NullValueHandling = NullValueHandling.Ignore })))
                    {

                        var dispositionHeader = new ContentDispositionHeaderValue("form-data");
                        dispositionHeader.Name = "knowledgeRequest";
                        jsonContent.Headers.ContentDisposition = dispositionHeader;
                        jsonContent.Headers.ContentType = null;

                        postContent.Add(jsonContent);

                        HttpResponseMessage response = await MakeRequestAsync(queryString, postContent);

                        // Get the client ID to use in the next request. See documentation for usage.

                        IEnumerable<string> values;
                        if (response.Headers.TryGetValues("X-MSEdge-ClientID", out values))
                        {
                            _clientIdHeader = values.FirstOrDefault();
                        }

                        // This example uses dictionaries instead of objects to access the response data.

                        var contentString = await response.Content.ReadAsStringAsync();
                        Dictionary<string, object> searchResponse = JsonConvert.DeserializeObject<Dictionary<string, object>>(contentString);

                        if (response.IsSuccessStatusCode)
                        {
                            PrintInsights(searchResponse);
                        }
                        else
                        {
                            PrintErrors(response.Headers, searchResponse);
                        }
                    }
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

Here's the HTTP request. It's your basic HTTP POST request. Use whatever HTTP client works for you.

```csharp
        // Makes the request to the Visual Search endpoint.

        static async Task<HttpResponseMessage> MakeRequestAsync(string queryString, MultipartFormDataContent postContent)
        {
            var client = new HttpClient();

            // Request headers. The subscription key is the only required header but you should
            // include User-Agent (especially for mobile), X-MSEdge-ClientID, X-Search-Location
            // and X-MSEdge-ClientIP (especially for local aware queries).

            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", _subscriptionKey);

            return (await client.PostAsync(_baseUri + queryString, postContent));
        }
```

That's all the more there is to sending a Visual Search request and getting back insights. To see what the JSON response looks like, see [Handling the insights search response](../../how-to/search-response.md).

The rest of the sections walk you through one way of parsing the JSON response and displaying the insights. Be sure to read the [use and display requirements](../../../bing-web-search/use-display-requirements.md) to make sure you comply with all display requirements.

For information about resizing the thumbnails, see [Resizing and cropping thumbnails](../../../bing-web-search/resize-and-crop-thumbnails.md).


## Displaying the insights

The response's `tags` field contains the list of insights. Each [tag](../../reference/response-objects.md#imagetag) includes a `displayName` field. The `displayName` field with the empty string contains the default insights that you're likely most interested in. For example, shopping sources, similar products, similar images, and more.

```csharp
        static void PrintInsights(Dictionary<string, object> response)
        {
            Console.WriteLine("The response contains the following insights:\n");

            var tags = response["tags"] as Newtonsoft.Json.Linq.JToken;

            foreach (Newtonsoft.Json.Linq.JToken tag in tags)
            {
                var displayName = (string)tag["displayName"];

                if (string.IsNullOrEmpty(displayName))
                {
                    // Handle default insights.

                    PrintDefaultInsights(tag);

                }
                else
                {
                    // Contains other tags related to the original search request that the 
                    // user might want to discover. Most will contain URLs to images or
                    // web search results but there are a couple that you might want to
                    // pursue like text recognition and entity.

                    Console.WriteLine("\nTag: {0}\n", (string)tag["displayName"]);
                    PrintOtherInsights(tag);
                }
            }
        }
```

## Displaying the default insights

The tag's `actions` field contains the list of insights. The action's `actionType` field tells you the insight that it contains. The `PrintDefaultInsights` method prints the insights you're most likely interested in. For a list of default insights, see [Default insights tag](../../how-to/search-response.md#default-insights-tag). 

```csharp
        static void PrintDefaultInsights(Newtonsoft.Json.Linq.JToken tag)
        {
            var actions = tag["actions"];

            foreach (Newtonsoft.Json.Linq.JToken action in actions)
            {
                if ((string)action["actionType"] == "PagesIncluding")
                {
                    Console.WriteLine("Webpages that include this image:\n");
                    DisplayPagesIncluding(action["data"]["value"]);
                }
                else if ((string)action["actionType"] == "VisualSearch")
                {
                    Console.WriteLine("Images that are visually similar to this image:\n");
                    DisplaySimilarImages(action["data"]["value"]);

                    // Capture offset if you're going to page by visually
                    // similar images.

                    //_nextOffset = (long)action["data"]["nextOffset"];
                }
                else if ((string)action["actionType"] == "ProductVisualSearch")
                {
                    Console.WriteLine("Images that have products that are visually similar to the products in this image:\n");
                    DisplaySimilarImages(action["data"]["value"]);

                    // Capture offset if you're going to page by visually 
                    // similar products.

                    //_nextOffset = (long)action["data"]["nextOffset"];
                }
                else if ((string)action["actionType"] == "ShoppingSources")
                {
                    Console.WriteLine("Sites where you can buy the product seen in this image:\n");
                    DisplayShoppingSources(action["data"]["offers"]);
                }
                else if ((string)action["actionType"] == "RelatedSearches")
                {
                    Console.WriteLine("Related search strings:\n");
                    DisplayRelatedSearches(action["data"]["value"]);
                }
                else
                {
                    Console.WriteLine("{0} - Not parsing\n", (string)action["actionType"]);
                }
            }
        }

```

### Parsing the default insights

```csharp

        // Displays all webpages that include the image.

        static void DisplayPagesIncluding(Newtonsoft.Json.Linq.JToken pages)
        {
            foreach (Newtonsoft.Json.Linq.JToken page in pages)
            {
                Console.WriteLine("\tWebpage: " + (string)page["hostPageUrl"]);
            }

            Console.WriteLine();
        }


        // Displays images that are visually similar to the source image.

        static void DisplaySimilarImages(Newtonsoft.Json.Linq.JToken images)
        {
            foreach (Newtonsoft.Json.Linq.JToken image in images)
            {
                DisplayImage(image);
            }

            Console.WriteLine();
        }


        // Displays websites where you can buy the product seen in the image.

        static void DisplayShoppingSources(Newtonsoft.Json.Linq.JToken offers)
        {
            foreach (Newtonsoft.Json.Linq.JToken offer in offers)
            {
                Console.WriteLine("\tSeller: {0} ({1})", offer["seller"]["name"], offer["url"]);
                Console.WriteLine("\tName: " + offer["name"]);
                Console.WriteLine("\tDescription: " + offer["description"]);
                Console.WriteLine("\tPrice: {0} {1}", offer["price"], offer["priceCurrency"]);
                Console.WriteLine("\tAvailability: " + offer["availability"]);
            }

            Console.WriteLine();
        }


        // Displays a single image.

        static void DisplayImage(Newtonsoft.Json.Linq.JToken image)
        {
            Console.WriteLine("\tThumbnail: " + image["thumbnailUrl"]);
            Console.WriteLine("\tThumbnail size: {0} (w) x {1} (h) ", image["thumbnail"]["width"], image["thumbnail"]["height"]);
            Console.WriteLine("\tOriginal image: " + image["contentUrl"]);
            Console.WriteLine("\tOriginal image size: {0} (w) x {1} (h) ", image["width"], image["height"]);
            Console.WriteLine("\tHost: {0} ({1})", image["hostPageDomainFriendlyName"], image["hostPageDisplayUrl"]);
            Console.WriteLine();
        }


        // Displays all related searchs.

        static void DisplayRelatedSearches(Newtonsoft.Json.Linq.JToken relatedSearches)
        {
            foreach (Newtonsoft.Json.Linq.JToken relatedSearch in relatedSearches)
            {
                Console.WriteLine("\tSearch string: {0} ({1})", (string)relatedSearch["displayText"], (string)relatedSearch["webSearchUrl"]);
            }

            Console.WriteLine();
        }
```


## Displaying other insights

The rest of the tags identify content that the user might want to explore. For example, if the image is of a hummingbird, the other tags might be Hummingbird Nest, Hummingbird Migration, or Hummingbird Species. The actions for these tags typically contain image links (ImageResults action type) or web search links (TextResults action type) that the user can use to explore more about the topic.

A tag may also include the Entity and TextRecognition action types. The tag might include the Entity insight if the image is of a well known person, place, or thing. To include entities, you must set `enableEntityData` to true. 

The tag might include the TextRecognition insight if the image contains text. 

```csharp
        static void PrintOtherInsights(Newtonsoft.Json.Linq.JToken tag)
        {
            var actions = tag["actions"];

            foreach (Newtonsoft.Json.Linq.JToken action in actions)
            {
                if ((string)action["actionType"] == "ImageResults")
                {
                    Console.WriteLine("\tImage search URLs:");
                    Console.WriteLine("\t\tDisplay name: " + action["displayName"]);
                    Console.WriteLine("\t\tImage API URL: " + action["serviceUrl"]);
                    Console.WriteLine("\t\tBing search URL: " + action["webSearchUrl"]);
                }
                else if ((string)action["actionType"] == "TextResults")
                {
                    Console.WriteLine("\tWeb search URLs:");
                    Console.WriteLine("\t\tDisplay name: " + action["displayName"]);
                    Console.WriteLine("\t\tWeb API URL: " + action["serviceUrl"]);
                    Console.WriteLine("\t\tBing search URL: " + action["webSearchUrl"]);
                }
                else if ((string)action["actionType"] == "Entity")
                {
                    DisplayEntity(action["data"]);
                }
                else if ((string)action["actionType"] == "TextRecognition")
                {
                    Console.WriteLine("\tRecognized text strings:\n");
                    DisplayRecognizedText(action["data"]["regions"]);
                }
                else
                {
                    Console.WriteLine("\tUnknown action type: {0}\n", (string)action["actionType"]);
                }
            }
        }
```

### Parsing the entity and text recognition insights

```csharp
        // Displays an entity.

        static void DisplayEntity(Newtonsoft.Json.Linq.JToken entity)
        {
            string rule = null;
            
            // Entities require attribution. Gets the list of attributions to apply.

            Dictionary<string, string> rulesByField = null;
            rulesByField = GetRulesByField(entity["contractualRules"]);

            Console.WriteLine("\tEntity\n");
            Console.WriteLine("\t\tName: " + entity["name"]);

            if (entity["image"] != null)
            {
                Console.WriteLine("\t\tImage: " + entity["image"]["thumbnailUrl"]);

                if (rulesByField.TryGetValue("image", out rule))
                {
                    Console.WriteLine("\t\t\tImage from: " + rule);
                }
            }

            if (entity["description"] != null)
            {
                Console.WriteLine("\t\tDescription: " + entity["description"]);

                if (rulesByField.TryGetValue("description", out rule))
                {
                    Console.WriteLine("\t\t\tData from: " + rulesByField["description"]);
                }
            }

            Console.WriteLine();
        }

        // Displays an entity.

        static void DisplayRecognizedText(Newtonsoft.Json.Linq.JToken regions)
        {
            foreach (Newtonsoft.Json.Linq.JToken region in regions)
            {
                foreach (Newtonsoft.Json.Linq.JToken line in region["lines"])
                {
                    Console.WriteLine("\t\tLine: " + line["text"]);
                }
            }
        }
```

### Handling contractual rules

The `GetRulesByField` method builds a dictionary of the rules that the calling method accesses when it displays the result. If the rule applies to the result as a whole, the key is `global`. Otherwise, the key is the name of the field that the rule targets (see the `targetPropertyName` field).

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

- For a more in depth web app example, see the [Visual search web app tutorial](../../tutorial/visual-search-single-page-app.md).

