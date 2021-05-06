---
title: Bing Spell check Python client library quickstart 
titleSuffix: Bing Search Services
description: The Spell check API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this Python quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-spellcheck
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Spell check Python client library

Use this quickstart to begin searching for news with the Bing Spell check client library for Python. While Bing Spell check has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-python/blob/main/samples/sdk/spellcheck_samples.py) with additional annotations, and features.


## Prerequisites

- [Python](https://www.python.org/) 2.x or 3.x
- The Bing Spell check client library for python

It is recommended that you use a python [virtual environment](https://docs.python.org/3/tutorial/venv.html). You can install and initialize a virtual environment with the [venv module](https://pypi.python.org/pypi/virtualenv). Install virtualenv for Python 2.7 with:

```console
python -m venv mytestenv
```

Install the Bing Spell check client library with:

```console
cd mytestenv
python -m pip install microsoft-bing-spellcheck
```

## Create and initialize the application

1. Create a new Python file in your favorite IDE or editor, and add the following import statements. 

    ```python
    from spell_check_client import SpellCheckClient
    from azure.core.credentials import AzureKeyCredential
    ```

2. Create a variable for your subscription key. 

    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    ```

## Create the search client

Create an instance of the `AzureKeyCredential`, and instantiate the client:

```python
 client = SpellCheckClient(AzureKeyCredential(subscription_key))
```

## Send a search request and get a response

1. Use `client.spell_checker()` with your search query to send a request to the Bing Spell check API, and get a response.

    ```python
    try:
        result = client.spell_checker("Bill Gatas", mode="proof")
        print("Correction for Query# \"bill gatas\"")
    ```

2. If the response contains search results, get the first one, and print its ID, name, and url.

    ```python

        if result.flagged_tokens:
            first_spellcheck_result = result.flagged_tokens[0]

            print("SpellCheck result count: {}".format(
                len(result.flagged_tokens)))
            print("First SpellCheck token: {}".format(
                first_spellcheck_result.token))
            print("First SpellCheck type: {}".format(
                first_spellcheck_result.type))
            print("First SpellCheck suggestion count: {}".format(
                len(first_spellcheck_result.suggestions)))

            if first_spellcheck_result.suggestions:
                first_suggestion = first_spellcheck_result.suggestions[0]
                print("First SpellCheck suggestion score: {}".format(
                    first_suggestion.score))
                print("First SpellCheck suggestion: {}".format(
                    first_suggestion.suggestion))
            else:
                print("Couldn't get any Spell check results!")

        else:
            print("Didn't see any SpellCheck results..")

    except Exception as err:
        print("Encountered exception. {}".format(err))
    ```


## See also 

- [What is the Bing Spell check API?](../../overview.md)
- [Bing Apis Python SDK samples](https://github.com/microsoft/bing-search-sdk-for-python/tree/main/samples)
