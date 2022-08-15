---
title: "Quickstart: Check spelling with the Bing Spell Check SDK for C#"
titleSuffix: Bing Search Services
description: The Spell Check API offers client libraries that makes it easy to integrate grammar capabilities into your applications. Use this C# quickstart to check a strings spelling and grammar.
services: bing-search-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Check spelling with Bing Spell Check SDK for C#

Use this quickstart to begin spell checking with Bing Spell Check SDK for C#. While Bing Spell Check has a REST API compatible with most programming languages, the SDK provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/samples/BingSearchSamples/BingAllSearch/Samples).

## Application dependencies

* Any edition of [Visual Studio 2017 or later](https://visualstudio.microsoft.com/downloads/).

To add the Bing Spell Check SDK to your project, select **Manage NuGet Packages** from **Solution Explorer** in Visual Studio. Add the `Microsoft.Bing.Language.SpellCheck` package. The package also installs the following dependencies:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

<!--
[!INCLUDE [bing-spell-check-signup-requirements](../../../../includes/bing-spell-check-signup-requirements.md)]
-->

## Create and initialize the application

1. Create a new C# console solution in Visual Studio. Then add the following `using` statement.
    
    ```csharp
    using System;
    using System.Linq;
    using Credentials;
    using Microsoft.Bing.SpellCheck;
    using Microsoft.Bing.SpellCheck.Models;
    ```

2. Create a new class. Then create an asynchronous function called `SpellCheckCorrection()` that takes a subscription key and sends the spell check request.

3. Instantiate the client by creating a new `ClientCredentials` object. 

    ```csharp
    var client = new SpellCheckClient(new ClientCredentials(subscriptionKey));
    ```

## Send the request and read the response

1. In the function created above, perform the following steps. Send the spell check request with the client. Add the text to be checked to the `text` parameter, and set the mode to `proof`.  
    
    ```csharp
    var result = client.SpellCheckerWithHttpMessagesAsync(text: "Bill Gatas",mode: "proof").Result;
    ```

2. Get the first spell check result, if there is one. Print the first misspelled word (token) returned, the token type, and the number of suggestions.

    ```csharp
    var firstspellCheckResult = result.Body.FlaggedTokens.FirstOrDefault();
    
    if (firstspellCheckResult != null)
    {
        Console.WriteLine("SpellCheck Results#{0}", result.Body.FlaggedTokens.Count);
        Console.WriteLine("First SpellCheck Result token: {0} ", firstspellCheckResult.Token);
        Console.WriteLine("First SpellCheck Result Type: {0} ", firstspellCheckResult.Type);
        Console.WriteLine("First SpellCheck Result Suggestion Count: {0} ", firstspellCheckResult.Suggestions.Count);
    }
    ```

3. Get the first suggested correction, if there is one. Print the suggestion score, and the suggested word. 

    ```csharp
    var suggestions = firstspellCheckResult.Suggestions;

    if (suggestions?.Count > 0)
    {
        var firstSuggestion = suggestions.FirstOrDefault();
        Console.WriteLine("First SpellCheck Suggestion Score: {0} ", firstSuggestion.Score);
        Console.WriteLine("First SpellCheck Suggestion : {0} ", firstSuggestion.Suggestion);
    }
    ```

## Run the application

Build and run your project. If you're using Visual Studio, press **F5** to debug the file.

## Next steps

> [!div class="nextstepaction"]
> [Create a single page web-app](../../tutorial/spellcheck.md)

- [What is the Bing Spell Check API?](../../overview.md)
- [Bing Spell Check C# SDK reference guide](https://github.com/microsoft/bing-search-sdk-for-net/tree/main/sdk/SpellCheck)
