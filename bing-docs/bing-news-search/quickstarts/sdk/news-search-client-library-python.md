---
title: Bing News Search Python client library quickstart 
titleSuffix: Bing Search Services
description: The News Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this Python quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing News Search Python client library

Use this quickstart to begin searching for news with the Bing News Search client library for Python. While Bing News Search has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-python/blob/main/samples/sdk/news_search_samples.py).

## Prerequisites

* [Python](https://www.python.org/) 2.x or 3.x

It is recommended to use a [virtual environment](https://docs.python.org/3/tutorial/venv.html) for your python development. You can install and initialize the virtual environment with the [venv module](https://pypi.python.org/pypi/virtualenv). You must install a virtualenv for Python 2.7. You can create a virtual environment with:

```console
python -m venv mytestenv
```

You can install the Bing News Search client library dependencies with this command:
    
```console
python -m pip install microsoft-bing-newssearch
```

<!--
[!INCLUDE [bing-news-search-signup-requirements](../../../../includes/bing-news-search-signup-requirements.md)]
-->

## Create and initialize the application

1. Create a new Python file in your favorite IDE or editor, and import the following libraries. Create a variable for your subscription key, and your search term.

    ```python
    from news_search_client import NewsSearchClient
    from azure.core.credentials import AzureKeyCredential
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    search_term = "Quantum Computing"
    ```

## Initialize the client and send a request

1. Create an instance of `AzureKeyCredential`.
    
    ```python
    client = NewsSearchClient(
        endpoint=ENDPOINT,
        credential=AzureKeyCredential(SUBSCRIPTION_KEY)
    )

    ```

2. Send a search query to the News Search API, store the response.

    ```python
    news_result = client.news.search(query=search_term, market="en-us", count=10)
    ```

## Parse the response

If any search results are found, print the first webpage result:

```python
if news_result.value:
    first_news_result = news_result.value[0]
    print("Total estimated matches value: {}".format(
        news_result.total_estimated_matches))
    print("News result count: {}".format(len(news_result.value)))
    print("First news name: {}".format(first_news_result.name))
    print("First news url: {}".format(first_news_result.url))
    print("First news description: {}".format(first_news_result.description))
    print("First published time: {}".format(first_news_result.date_published))
    print("First news provider: {}".format(first_news_result.provider[0].name))
else:
    print("Didn't see any news result data..")
```

## Next steps

> [!div class="nextstepaction"]
> [Create a single-page web app](../../tutorial/bing-news-search-single-page-app.md)
