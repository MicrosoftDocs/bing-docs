---
title: Bing Entity Search Python client library quickstart 
titleSuffix: Bing Search Services
description: The Entity Search API offers client libraries that makes it easy to integrate search capabilities into your applications. Use this Python quickstart to send search requests and get back results.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 07/15/2020
ms.author: scottwhi
---

# Quickstart: Use the Bing Entity Search Python client library

Use this quickstart to begin searching for entities with the Bing Entity Search client library for Python. While Bing Entity Search has a REST API compatible with most programming languages, the client library provides an easy way to integrate the service into your applications. The source code for this sample can be found on [GitHub](https://github.com/microsoft/bing-search-sdk-for-python/blob/main/samples/sdk/entity_search_samples.py).

## Prerequisites

* Python [2.x or 3.x](https://www.python.org/)

* The [Bing Entity Search SDK for Python](https://pypi.org/project/microsoft-bing-entitysearch/)

It is recommended that you use a python virtual environment. You can install and initialize a virtual environment with the venv module. You can install virtualenv with:

```Console
python -m venv mytestenv
```

Install the Bing Entity Search client library with:

```Console
cd mytestenv
python -m pip install microsoft-bing-entitysearch
```

<!--
[!INCLUDE [bing-news-search-signup-requirements](../../../..//includes/bing-entity-search-signup-requirements.md)]
-->

## Create and initialize the application

1. Create a new Python file in your favorite IDE or editor, and add the following import statements. 

    ```python
    from entity_search_client import EntitySearchClient
    from entity_search_client.models import Place
    from azure.core.credentials import AzureKeyCredential
    ```

2. Create a variable for your subscription key and endpoint. Instantiate the client by creating a new `AzureKeyCredential` object with your key.
    
    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    client = EntitySearchClient(
        endpoint=ENDPOINT,
        credential=AzureKeyCredential(SUBSCRIPTION_KEY)
    )
    ```

## Send a search request and receive a response

1. Send a search request to Bing Entity Search with `client.entities.search()` and a search query. 
    
    ```python
    entity_data = client.entities.search(query="Gibralter")
    ```

2. If entities were returned, convert `entity_data.entities.value` to a list, and print the first result.
    ```python
    if entity_data.entities.value:
    
        main_entities = [entity for entity in entity_data.entities.value
                         if entity.entity_presentation_info.entity_scenario == "DominantEntity"]
    
        if main_entities:
            print(main_entities[0].description)
    ```

## Next steps

> [!div class="nextstepaction"]
> [Create a single-page web app](../../tutorial/bing-entities-search-single-page-app.md)

* [What is the Bing Entity Search API?](../../overview.md )