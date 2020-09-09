---
title: "Quickstart: Perform a web search with C# - Bing Web Search API"
titleSuffix: Bing Search Services
description: Use this quickstart to send requests to the Bing Web Search API using C#.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Search the web using Bing Web Search API and C#

Use this quickstart to make your first call to Bing Web Search API. This C# console application sends a search request to Bing and parses the response. Since it's a console application, it displays a text-based version of the response for illustrative purposes only. 

Grab your favorite .NET editor, JSON library, and [subscription key](../../get-subscription-key.md) for Bing Web Search and let's get started. 


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

Add a namespace and class. This example uses `WebSearchQuickstart` for the namespace and `Program` for the class.  

```csharp
namespace WebSearchQuickstart
{
    class Program
    {
        // The code in the following sections goes here.
    }
}
```

## Define variables

Add a few variables to the `Program` class. For simplicity, this example hardcodes the subscription key, but you should make sure you're pulling it from secured storage instead.

```csharp
        // In production, make sure you're pulling the subscription key from secured storage.

        private static string _subscriptionKey = "<your key goes here"; 
        private static string _baseUri = "https://api.bing.microsoft.com/v7.0/search";

        // The user's search string.

        private static string searchString = "coronovirus vaccine";
```

Here's all the query parameters you can add to the base URI. The *q* parameter is required and you should always include the *mkt* parameter too. The rest are optional. For information about these parameters, see [Query parameters](../../reference/query-paramters.md).

```csharp
        private const string QUERY_PARAMETER = "?q=";  // Required
        private const string MKT_PARAMETER = "&mkt=";  // Strongly suggested
        private const string RESPONSE_FILTER_PARAMETER = "&responseFilter=";
        private const string COUNT_PARAMETER = "&count=";
        private const string OFFSET_PARAMETER = "&offset=";
        private const string FRESHNESS_PARAMETER = "&freshness=";
        private const string SAFE_SEARCH_PARAMETER = "&safeSearch=";
        private const string TEXT_DECORATIONS_PARAMETER = "&textDecorations=";
        private const string TEXT_FORMAT_PARAMETER = "&textFormat=";
        private const string ANSWER_COUNT = "&answerCount=";
        private const string PROMOTE = "&promote=";
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
                queryString += MKT_PARAMETER + "en-us";
                //queryString += RESPONSE_FILTER_PARAMETER + Uri.EscapeDataString("webpages,news");
                queryString += TEXT_DECORATIONS_PARAMETER + Boolean.TrueString;

                HttpResponseMessage response = await MakeRequestAsync(queryString);

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

That's all the more there is to sending a search request and getting back search results. To see what all the answers look like in the JSON response, see [Handling the web search response](../../search-responses.md).

The rest of the sections walk you through one way of parsing the JSON response and displaying the search results. Be sure to read the [use and display requirements](../../use-display-requirements.md) to make sure you comply with all display requirements.


## Using ranking to display the search results

If the request succeeds, the code calls the `PrintResponse` method to print the search results in the console window.

The example uses the RankingResponse answer to display the search results. The ranking determines which answers to display in the pole, mainline, and sidebar sections of the search results page. For information about using the RankingResponse answer, see [Use ranking to display search results](../../rank-results.md). 

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

Each item in the ranking tells you whether to display all results from the answer together or to display a single result from the answer.

If the item includes the `resultIndex` field, use the index value to display that single result from the answer. But if the item doesn't include `resultIndex`, you display all results from the answer. Typically, the ranking has you display individual webpages interspersed amongst the other answers, and has you group all images together. For answers that have many results like images and videos, you typically display a few of the images or videos and provide a link that users can click to see the rest.

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
                else if ("Images" == answerType)
                {
                    if ((index = item["resultIndex"]) == null)
                    {
                        DisplayAllImages(((Newtonsoft.Json.Linq.JToken)response["images"])["value"]);
                    }
                    else
                    {
                        DisplayImage(((Newtonsoft.Json.Linq.JToken)response["images"])["value"].ElementAt((int)index));
                    }
                }
                else if ("Videos" == answerType)
                {
                    if ((index = item["resultIndex"]) == null)
                    {
                        DisplayAllVideos(((Newtonsoft.Json.Linq.JToken)response["videos"])["value"]);
                    }
                    else
                    {
                        DisplayVideo(((Newtonsoft.Json.Linq.JToken)response["videos"])["value"].ElementAt((int)index));
                    }
                }
                else if ("News" == answerType)
                {
                    if ((index = item["resultIndex"]) == null)
                    {
                        DisplayAllNews(((Newtonsoft.Json.Linq.JToken)response["news"])["value"]);
                    }
                    else
                    {
                        DisplayArticle(((Newtonsoft.Json.Linq.JToken)response["news"])["value"].ElementAt((int)index));
                    }
                }
                else if ("RelatedSearches" == answerType)
                {
                    if ((index = item["resultIndex"]) == null)
                    {
                        DisplayAllRelatedSearches(((Newtonsoft.Json.Linq.JToken)response["relatedSearches"])["value"]);
                    }
                    else
                    {
                        DisplayRelatedSearch(((Newtonsoft.Json.Linq.JToken)response["relatedSearches"])["value"].ElementAt((int)index));
                    }
                }
                else if ("Entities" == answerType)
                {
                    if ((index = item["resultIndex"]) == null)
                    {
                        DisplayAllEntities(((Newtonsoft.Json.Linq.JToken)response["entities"])["value"]);
                    }
                    else
                    {
                        DisplayEntity(((Newtonsoft.Json.Linq.JToken)response["entities"])["value"].ElementAt((int)index));
                    }
                }
                else if ("Places" == answerType)
                {
                    if ((index = item["resultIndex"]) == null)
                    {
                        DisplayAllPlaces(((Newtonsoft.Json.Linq.JToken)response["places"])["value"]);
                    }
                    else
                    {
                        DisplayPlace(((Newtonsoft.Json.Linq.JToken)response["places"])["value"].ElementAt((int)index));
                    }
                }
                else if ("Computation" == answerType)
                {
                    DisplayComputation((Newtonsoft.Json.Linq.JToken)response["computation"]);
                }
                else if ("Translations" == answerType)
                {
                    DisplayTranslations((Newtonsoft.Json.Linq.JToken)response["translations"]);
                }
                else if ("TimeZone" == answerType)
                {
                    DisplayTimeZone((Newtonsoft.Json.Linq.JToken)response["timeZone"]);
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

            if (null != rulesByField)
            {
                try
                {
                    Console.WriteLine("\t\t\tData from: " + rulesByField["snippet"]);
                }
                catch (Exception) { };
            }

            Console.WriteLine();

        }

        // Displays all images in the Images answer.

        static void DisplayAllImages(Newtonsoft.Json.Linq.JToken images)
        {
            foreach (Newtonsoft.Json.Linq.JToken image in images)
            {
                DisplayImage(image);
            }
        }

        // Displays a single image.

        static void DisplayImage(Newtonsoft.Json.Linq.JToken image)
        {
            Console.WriteLine("\tImage\n");
            Console.WriteLine("\t\tThumbnail: " + image["thumbnailUrl"]);
            Console.WriteLine();
        }

        // Displays all videos in the Videos answer.

        static void DisplayAllVideos(Newtonsoft.Json.Linq.JToken videos)
        {
            foreach (Newtonsoft.Json.Linq.JToken video in videos)
            {
                DisplayVideo(video);
            }
        }

        // Displays a single video.

        static void DisplayVideo(Newtonsoft.Json.Linq.JToken video)
        {
            Console.WriteLine("\tVideo\n");
            Console.WriteLine("\t\tEmbed HTML: " + video["embedHtml"]);
            Console.WriteLine();
        }

        // Displays all news articles in the News answer.

        static void DisplayAllNews(Newtonsoft.Json.Linq.JToken news)
        {
            foreach (Newtonsoft.Json.Linq.JToken article in news)
            {
                DisplayArticle(article);
            }
        }

        // Displays a single news article.

        static void DisplayArticle(Newtonsoft.Json.Linq.JToken article)
        {
            // News articles require attribution. Gets the list of attributions to apply.

            Dictionary<string, string> rulesByField = null;
            rulesByField = GetRulesByField(article["contractualRules"]);

            Console.WriteLine("\tArticle\n");
            Console.WriteLine("\t\tName: " + article["name"]);
            Console.WriteLine("\t\tURL: " + article["ulr"]);
            Console.WriteLine("\t\tDescription: " + article["description"]);
            Console.WriteLine("\t\tArticle from: " + rulesByField["global"]);
            Console.WriteLine();
        }

        // Displays all related search in the RelatedSearches answer.

        static void DisplayAllRelatedSearches(Newtonsoft.Json.Linq.JToken searches)
        {
            foreach (Newtonsoft.Json.Linq.JToken search in searches)
            {
                DisplayRelatedSearch(search);
            }
        }

        // Displays a single related search query.

        static void DisplayRelatedSearch(Newtonsoft.Json.Linq.JToken search)
        {
            Console.WriteLine("\tRelatedSearch\n");
            Console.WriteLine("\t\tName: " + search["displayText"]);
            Console.WriteLine("\t\tURL: " + search["webSearchUrl"]);
            Console.WriteLine();
        }

        // Displays all entities in the Entities answer.

        static void DisplayAllEntities(Newtonsoft.Json.Linq.JToken entities)
        {
            foreach (Newtonsoft.Json.Linq.JToken entity in entities)
            {
                DisplayEntity(entity);
            }
        }

        // Displays a single entity.

        static void DisplayEntity(Newtonsoft.Json.Linq.JToken entity)
        {
            // Entities require attribution. Gets the list of attributions to apply.

            Dictionary<string, string> rulesByField = null;
            rulesByField = GetRulesByField(entity["contractualRules"]);

            Console.WriteLine("\tEntity\n");
            Console.WriteLine("\t\tName: " + entity["name"]);

            if (entity["image"] != null)
            {
                Console.WriteLine("\t\tImage: " + entity["image"]["thumbnail"]);

                try
                {
                    Console.WriteLine("\t\t\tImage from: " + rulesByField["image"]);
                }
                catch (Exception) { };
            }

            if (entity["description"] != null)
            {
                Console.WriteLine("\t\tDescription: " + entity["description"]);

                try
                {
                    Console.WriteLine("\t\t\tData from: " + rulesByField["description"]);
                }
                catch (Exception) { };
            }
            else
            {
                // See if presentation info can shed light on what this entity is.

                var hintCount = entity["entityPresentationInfo"]["entityTypeHints"].Count();
                Console.WriteLine("\t\tEntity hint: " + entity["entityPresentationInfo"]["entityTypeHints"][hintCount - 1]);
            }

            Console.WriteLine();
        }

        // Displays all places in the Places answer.

        static void DisplayAllPlaces(Newtonsoft.Json.Linq.JToken places)
        {
            foreach (Newtonsoft.Json.Linq.JToken place in places)
            {
                DisplayPlace(place);
            }
        }

        // Displays a single place.

        static void DisplayPlace(Newtonsoft.Json.Linq.JToken place)
        {
            Console.WriteLine("\tPlace\n");
            Console.WriteLine("\t\tName: " + place["name"]);
            Console.WriteLine("\t\tPhone: " + place["telephone"]);
            Console.WriteLine("\t\tWebsite: " + place["url"]);
            Console.WriteLine();
        }

        // Displays the Computation answer.

        static void DisplayComputation(Newtonsoft.Json.Linq.JToken expression)
        {
            Console.WriteLine("\tComputation\n");
            Console.WriteLine("\t\t{0} is {1}", expression["expression"], expression["value"]);
            Console.WriteLine();
        }

        // Displays the Translation answer.

        static void DisplayTranslations(Newtonsoft.Json.Linq.JToken translation)
        {
            // Some webpages require attribution. Checks if this page requires
            // attribution and gets the list of attributions to apply.

            Dictionary<string, string> rulesByField = null;
            rulesByField = GetRulesByField(translation["contractualRules"]);

            // The translatedLanguageName field contains a 2-character language code,
            // so you might want to provide the means to print Spanish instead of es.

            Console.WriteLine("\tTranslation\n");
            Console.WriteLine("\t\t\"{0}\" translates to \"{1}\" in {2}", translation["originalText"], translation["translatedText"], translation["translatedLanguageName"]);
            Console.WriteLine("\t\tTranslation by " + rulesByField["global"]);
            Console.WriteLine();
        }

        // Displays the TimeZone answer. This answer has multiple formats, so you need to figure
        // out which fields exist in order to format the answer.

        static void DisplayTimeZone(Newtonsoft.Json.Linq.JToken timeZone)
        {
            Console.WriteLine("\tTime zone\n");

            if (timeZone["primaryCityTime"] != null)
            {
                var time = DateTime.Parse((string)timeZone["primaryCityTime"]["time"]);
                Console.WriteLine("\t\tThe time in {0} is {1}:", timeZone["primaryCityTime"]["location"], time);

                if (timeZone["otherCityTimes"] != null)
                {
                    Console.WriteLine("\t\tThere are {0} other time zones", timeZone["otherCityTimes"].Count());
                }
            }

            if (timeZone["date"] != null)
            {
                Console.WriteLine("\t\t" + timeZone["date"]);
            }

            if (timeZone["primaryResponse"] != null)
            {
                Console.WriteLine("\t\t" + timeZone["primaryResponse"]);
            }

            if (timeZone["timeZoneDifference"] != null)
            {
                Console.WriteLine("\t\t{0} {1}", timeZone["description"], timeZone["timeZoneDifference"]["text"]);
            }

            if (timeZone["primaryTimeZone"] != null)
            {
                Console.WriteLine("\t\t" + timeZone["primaryTimeZone"]["timeZoneName"]);
            }

            Console.WriteLine();
        }
```

### Handling contractual rules

You need to check each result to see if it includes one or more contractual rules. Some webpages contain contractual rules as do news articles, entities, and translations. Some rules apply to the result as a whole and others apply to a specific field. If the rule applies to a specific field, it includes the `targetPropertyName` field, which contains the name of the target field. 

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

- For a more in depth web app example, see the [Web Search tutorial](../../tutorial/bing-web-search-single-page-app.md).

