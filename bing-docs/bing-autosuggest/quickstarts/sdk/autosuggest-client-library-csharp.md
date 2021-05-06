---
title: Bing Autosuggest C# client library quickstart 
titleSuffix: Bing Search Services
description: The Autosuggest API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this C# quickstart to send a partial search term and get back suggested terms.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-autosuggest
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Autosuggest .NET client library

Get started with the Bing Autosuggest client library for .NET. Follow these steps to install the package and try out the example code for basic tasks.

Use the Bing Autosuggest client library for .NET to get search suggestions based on partial query strings.

[Reference documentation](https://docs.microsoft.com/en-us/bing/search-apis/bing-autosuggest/overview) | [Library source code](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/sdk/AutoSuggest) | [Sample code](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/samples/BingSearchSamples/BingAutoSuggest)

## Prerequisites

* An Azure subscription. If you don't already have an Azure subscription, [you can create one for free](https://azure.microsoft.com/free/).
* The current version of [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

<!--
[!INCLUDE [bing-autosuggest-signup-requirements](../../../../includes/bing-autosuggest-signup-requirements.md)]
-->

## Create environment variables

>[!NOTE]
> The endpoints for resources created after July 1, 2019 use the custom subdomain format shown below. For more information and a complete list of regional endpoints,

Using your key and endpoint from the resource you created, create two environment variables for authentication:
<!-- replace the below variable names with the names expected in the code sample.-->
* `AUTOSUGGEST_SUBSCRIPTION_KEY`: The resource key for authenticating your requests.
* `AUTOSUGGEST_ENDPOINT`: The resource endpoint for sending API requests.  

Use the instructions for your operating system.
<!-- replace the below endpoint and key examples -->
### [Windows](#tab/windows)

```console
setx AUTOSUGGEST_SUBSCRIPTION_KEY <replace-with-your-autosuggest-api-key>
setx AUTOSUGGEST_ENDPOINT <replace-with-your-autosuggest-api-endpoint>
```

After you add the environment variable, restart the console window.

### [Linux](#tab/linux)

```bash
export AUTOSUGGEST_SUBSCRIPTION_KEY=<replace-with-your-autosuggest-api-key>
export AUTOSUGGEST_ENDPOINT=<replace-with-your-autosuggest-api-endpoint>
```

After you add the environment variable, run `source ~/.bashrc` from your console window to make the changes effective.

### [macOS](#tab/unix)

Edit your `.bash_profile`, and add the environment variable:

```bash
export AUTOSUGGEST_SUBSCRIPTION_KEY=<replace-with-your-autosuggest-api-key>
export AUTOSUGGEST_ENDPOINT=<replace-with-your-autosuggest-api-endpoint>
```

After you add the environment variable, run `source .bash_profile` from your console window to make the changes effective.
***

## Create a new C# application

Create a new .NET Core application in your preferred editor or IDE. 

In a console window (such as cmd, PowerShell, or Bash), use the `dotnet new` command to create a new console app with the name `bing-autosuggest-quickstart`. This command creates a simple "Hello World" C# project with a single source file: *program.cs*. 

```console
dotnet new console -n bing-autosuggest-quickstart
```

Change your directory to the newly created app folder. You can build the application with:

```console
dotnet build
```

The build output should contain no warnings or errors. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

From the project directory, open the *program.cs* file in your preferred editor or IDE. Add the following `using` directives:

```csharp
    using System;
    using Microsoft.Bing.AutoSuggest;
    using Microsoft.Bing.AutoSuggest.Models;
    using Microsoft.Rest;
    using Microsoft.Rest.Serialization;
    using Newtonsoft.Json;
    using System.Collections;
    using System.Collections.Generic;
    using System.Net;
    using System.Net.Http;
    using System.Threading;
    using System.Threading.Tasks;
    using Credentials;
```


# Create a client and send a search request

1. Create a new search client. Add your subscription key by creating a new `ClientCredentials`.

    ```csharp
    var client = new AutoSuggestClient(new ClientCredentials(subscriptionKey));
    ```

1. Use the client's `AutoSuggestMethod` function to search for your query:
    
    ```csharp
    var suggestions = client.AutoSuggestMethod(query: "Satya Nadella");
    ```

## Get and print an entity description



    ```csharp
    if (suggestions != null && suggestions.SuggestionGroups.Count > 0)
    {
        // dump content
        Console.WriteLine("Searched for \"Satya Nadella\" and found suggestions:");
        foreach (var suggestion in suggestions.SuggestionGroups[0].SearchSuggestions)
        {
            Console.WriteLine("....................................");
            Console.WriteLine(suggestion.Query);
            Console.WriteLine(suggestion.DisplayText);
            Console.WriteLine(suggestion.Url);
            Console.WriteLine(suggestion.SearchKind);
            
        }
    }
    else
    {
        Console.WriteLine("Didn't see any suggestion..");
    }
    ```




## Run the application

Run the application from your application directory with the `dotnet run` command.

```dotnet
dotnet run
```


## Next steps

> [!div class="nextstepaction"]
> [Bing Autosuggest tutorial](../../tutorial/autosuggest.md)

## See also

- [What is Bing Autosuggest?](../../overview.md)
- [Bing Autosuggest .NET reference](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/sdk/AutoSuggest)
