---
title: Bing Autosuggest Python client library quickstart 
titleSuffix: Bing Search Services
description: The Autosuggest API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this Python quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-autosuggest
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Autosuggest Python client library

Use this quickstart to begin searching for suggestions with the Bing Autosuggest client library for Python. While Bing Autosuggest has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-python/blob/main/samples/sdk/autosuggest_samples.py).

## Prerequisites

* Python [2.x or 3.x](https://www.python.org/)

* The [Bing Autosuggest SDK for Python](https://pypi.org/project/microsoft-bing-autosuggest/)

It is recommended that you use a python virtual environment. You can install and initialize a virtual environment with the venv module. You can install virtualenv with:

```Console
python -m venv mytestenv
```

Install the Bing Autosuggest client library with:

```Console
cd mytestenv
python -m pip install microsoft-bing-autosuggest
```

<!--
[!INCLUDE [bing-news-search-signup-requirements](../../../..//includes/bing-autosuggest-signup-requirements.md)]
-->

## Create and initialize the application

1. Create a new Python file in your favorite IDE or editor, and add the following import statements. 

    ```python
    from autosuggest_client import AutoSuggestClient
    from autosuggest_client.models import Suggestions, SuggestionsSuggestionGroup, SearchAction,ErrorResponse
    from azure.core.credentials import AzureKeyCredential
    ```

2. Create a variable for your subscription key and endpoint. Instantiate the client by creating a new `AzureKeyCredential` object with your key.
    
    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    client = AutoSuggestClient(
        endpoint=ENDPOINT,
        credential=AzureKeyCredential(SUBSCRIPTION_KEY)
    )
    ```

## Send a search request and receive a response

1. Send a search request to Bing Autosuggest with `client.entities.search()` and a search query. 
    
    ```python
    suggestions = client.auto_suggest(
            query="Satya Nadella")  # type: Suggestions
    ```

2. If suggestions were returned, convert `suggestions.suggestion_groups` to a list, and print the result.
    ```python
    if suggestions.suggestion_groups:
            print("Searched for \"Satya Nadella\" and found suggestions:")
            suggestion_group = suggestions.suggestion_groups[0]  # type: SuggestionsSuggestionGroup
            for suggestion in suggestion_group.search_suggestions:  # type: SearchAction
                print("....................................")
                print(suggestion.query)
                print(suggestion.display_text)
                print(suggestion.url)
                print(suggestion.search_kind)
        else:
            print("Didn't see any suggestion..")
    ```

## Next steps

> [!div class="nextstepaction"]
> [Create a single-page web app](../../tutorial/autosuggest.md)

* [What is the Bing Autosuggest API?](../../overview.md )