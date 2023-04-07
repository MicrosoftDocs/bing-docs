---
title: Define Custom Autosuggest suggestions - Bing Custom Search
titleSuffix: Bing Search Services
description: Custom Autosuggest returns a list of suggested search query strings that are relevant to your search experience.
services: bing-search-services
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 04/07/2023
---

# Configure your custom autosuggest experience

Use Custom Autosuggest API to improve your users' search box experience by providing a list of suggested query strings with each character they type. The list will contain a maximum of 10 suggestions.

As the user types their search query, send Bing the partial query string and get back suggestions. The more complete the user’s query string is, the more relevant the list of suggested query terms will be. For example, the suggestions that Bing might return for *m* are likely to be less relevant than the suggestions Bing returns for *micro*.

You specify whether to return custom search string suggestions and/or Bing suggestions. If you include Bing suggestions, custom suggestions appear before the Bing suggestions. If you provide enough relevant suggestions, it's possible that the returned list of suggestions will not include Bing suggestions. Bing suggestions are always in the context of your Custom Search instance.

>[!NOTE]
>
> - It may take up to 24 hours for Custom Autosuggest configuration changes to take effect.
> - To use this feature, you must subscribe to Custom Search at the appropriate level.

## Get started with this feature

To get started configuring search query suggestions for your instance:

1. Open a browser and navigate to the [Bing Custom Search portal](https://customsearch.ai).  

1. Select an instance to add suggested query strings to.  

1. Click the **Autosuggest** tab.  

### Enable Bing suggestions

>[!NOTE]
> Custom Search AutoSuggest only supports **en-US** market.

To enable Bing suggestions, toggle the **Automatic Bing suggestions** slider to the **On** position.

### Add your own suggestions

To add your own query string suggestions, add them to the list under **User-defined suggestions**. After adding a suggestion in the list, press the Enter key or click the **+** icon. You can specify the suggestion in any language. You can add a maximum of 5,000 query string suggestions.

### Upload suggestions

As an option, you can upload a list of suggestions from a file. The file must contain one search query string per line. To upload the file, click the Upload icon and select the file to upload. The service extracts the suggestions from the file and adds them to the list.

### Remove suggestions

To remove a query string suggestion, click the Trash Bin icon next to the suggestion you want to remove.

### Block suggestions

To specify a list of search strings that you don't want Bing to return, click **Show blocked suggestions**. Add the query string to the list and press the Enter key or click the **+** icon. You can add a maximum of 50 blocked query strings.

### Publish or revert

Your Autosuggest configuration won't take affect until you publish the instance.

Custom search has two environments: staging/testing (see the **Configuration** tab) and production (see the **Production** tab). When you create a new instance or make changes to an existing instance, those changes occur in the testing environment.

After validating that your changes and are ready to publish, click **Publish**. Changes are not reflected against your production endpoints until you publish.

Before publishing, if you decide that you don't want to keep the changes you've made, click **Revert**. When you revert your changes, the **Published** version remains unchanged and the **Configuration** version is reverted to match the **Published** version.

>[!NOTE]  
>It may take up to 24 hours for Custom Autosuggest configuration changes to take effect.

## Enabling Autosuggest in Hosted UI

To enable query string suggestions for your hosted UI, click **Hosted UI**. Scroll down to the **Additional Configuration** section. Under **Web search**, toggle **Enable autosuggest** to **On**. To enable Autosuggest, you must select a layout that includes a search box.

## Calling the Autosuggest API

To get suggested query strings using the Bing Custom Search API, send a `GET` request to the following endpoint.

`https://api.bing.microsoft.com/v7.0/custom/suggestions/search`

Add query parameters as appropriate. The request must include the *q* and *customConfig* query parameters.

The response contains a list of `SearchAction` objects that contain the suggested query strings.

```json
        {  
            "displayText" : "sailing lessons seattle",  
            "query" : "sailing lessons seattle",  
            "searchKind" : "CustomSearch"  
        },  
```

Each suggestion includes a `displayText` and `query` field. The `displayText` field contains the suggested query string that you use to populate your search box's dropdown list.

If the user selects a suggested query string from the dropdown list, use the query string in the `query` field when calling [Bing Custom Search API](../overview.md).
