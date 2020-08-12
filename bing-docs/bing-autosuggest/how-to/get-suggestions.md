---
title: Suggesting search terms with the Bing Autosuggest API
titleSuffix: Bing Search Services
description: Use Bing Autosuggest API to improve your users' search box experience by providing a list of suggested query strings with each character they type.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Getting query string suggestions

Use Bing Autosuggest API to improve your users' search box experience by providing a list of suggested query strings with each character they type.

As the user types their search query, send Bing the partial query string and get back suggestions. The more complete the user’s query string is, the more relevant the list of suggested query terms will be. For example, the suggestions that Bing might return for *s* are likely to be less relevant than the suggestions Bing returns for *sail*. 

![Autosuggest drop-down search box list](media/bing-autosuggest-drop-down-list.PNG)

The suggestions are based on user intent and past searches made by the user and others.

When the user selects a suggestion from the drop-down list, you can use it to search the web using one of [Bing Search APIs](../../bing-web-search/bing-api-comparison.md) or you can send the user to Bing's search results page for the query.


## Sending a request

Sending a request is easy. If you have your subscription key, just send an HTTP get request to the following endpoint:

```
https://api.bing.microsoft.com/bing/v7.0/suggestions
```

Here's a cURL example that shows you how to call the endpoint using your subscription key. Change the *q* query parameter to get query string suggestions for whatever you'd like.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/suggestions?q=sail
```

## Request and response headers

Besides the required subscription key header, Bing does suggest you include a couple of other headers to provide a better search experience for your user. Those headers include:

- User-Agent &mdash; Lets Bing know whether needs a mobile or desktop experience.
- X-MSEdge-ClientID &mdash; Provides continuity of experience.
- X-MSEdge-ClientIP &mdash; Provides the user's location for location aware queries.
- X-Search-Location &mdash; Provides the user's location for location aware queries.

The more information you can provide Bing, the better the search experience will be for your users. To learn more about these headers, see [Request headers](reference/headers.md#request-headers).

Here's a cURL example that includes these headers.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" -H "X-MSEdge-ClientID: 00B4230B74496E7A13CC2C1475056FF4" -H "X-MSEdge-ClientIP: 11.22.33.44" -H "X-Search-Location: lat:55;long:-111;re:22" -A "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.65 Safari/537.36" https://api.bing.microsoft.com/bing/v7.0/suggestions?q=sail
```

Bing returns a couple of headers you should capture. 

- BingAPIs-TraceId &mdash; ID that identifies the request in the log file.
- X-MSEdge-ClientID &mdash; The ID that you need to pass in subsequent request to provide continuity of experience.
- BingAPIs-Market &mdash; The market used by Bing for the request.

To learn more about these headers, see [Response headers](reference/headers.md#response-headers).

Here's a cURL call that returns the response headers. If you want to remove the response data so you can see only the headers, include the `-o nul` parameter.

```curl
curl -D - -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/suggestions?q=sail
```


## Query parameters

The only query parameter that you must pass is the *q* parameter, which you set to the user's query string. You must URL encode the user's query string and all query parameter values that you pass.

The only other query parameter that you should set is the *mkt* parameter. This parameter specifies the market where the results come from, which is typically the market where the user is making the request from.

To learn more about these parameters and other parameters that you may specify, see [Query parameters](reference/query-parameters.md).

Here's a cURL example that includes these query parameters.

```curl
curl -H "Ocp-Apim-Subscription-Key: <yourkeygoeshere>" https://api.bing.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us
```

## Handling the response

The body of the response is a [Suggestions](reference/response-objects.md#suggestions) object. Use the suggestions from the Web suggestions group. The `searchSuggestions` list contains at most eight suggestions. You must display all suggestions in the order provided. The list is in order of decreasing relevance. The first suggestion is the most relevant and the last suggestion is the least relevant.

```json
{
  "_type" : "Suggestions",
  "queryContext" : {
    "originalQuery" : "sail"
  },
  "suggestionGroups" : [
    {
      "name" : "Web",
      "searchSuggestions" : [
        {
          "url" : "https://www.bing.com/search?q=sailing+lessons+seattle&FORM=USBAPI",
          "displayText" : "sailing lessons seattle",
          "query" : "sailing lessons seattle",
          "searchKind" : "WebSearch"
        },
        {
          "url" : "https://www.bing.com/search?q=sailor+moon+news&FORM=USBAPI",
          "displayText" : "sailor moon news",
          "query" : "sailor moon news",
          "searchKind" : "WebSearch"
        },
        {
          "url" : "https://www.bing.com/search?q=sailor+jack%27s+lincoln+city&FORM=USBAPI",
          "displayText" : "sailor jack's lincoln city",
          "query" : "sailor jack's lincoln city",
          "searchKind" : "WebSearch"
        },
        {
          "url" : "https://www.bing.com/search?q=sailing+anarchy&FORM=USBAPI",
          "displayText" : "sailing anarchy",
          "query" : "sailing anarchy",
          "searchKind" : "WebSearch"
        },
        {
          "url" : "https://www.bing.com/search?q=sailboats+for+sale&FORM=USBAPI",
          "displayText" : "sailboats for sale",
          "query" : "sailboats for sale",
          "searchKind" : "WebSearch"
        },
        {
          "url" : "https://www.bing.com/search?q=sailstn.mylabsplus.com&FORM=USBAPI",
          "displayText" : "sailstn.mylabsplus.com",
          "query" : "sailstn.mylabsplus.com",
          "searchKind" : "WebSearch"
        },
        {
          "url" : "https://www.bing.com/search?q=sailusfood&FORM=USBAPI",
          "displayText" : "sailusfood",
          "query" : "sailusfood",
          "searchKind" : "WebSearch"
        },
        {
          "url" : "https://www.bing.com/search?q=sailboats+for+sale+seattle&FORM=USBAPI",
          "displayText" : "sailboats for sale seattle",
          "query" : "sailboats for sale seattle",
          "searchKind" : "WebSearch"
        }
      ]
    }
  ]
}
```

The [SearchAction](../reference/response-objects.md#searchaction) object contains the suggested query string. Use the `displayText` field to populate your search box's drop-down list and the `query` when calling one of Bing's Search APIs such as [Bing Web Search API](../../bing-web-search/overview.md).

If you don't want to call one of Bing's Search APIs, you can use the URL in the `url` field to send the user to the Bing search results page instead.


## Next steps

- Learn about the [quickstarts](quickstarts/quickstarts.md) and [samples](samples.md) that are available to help you get up and running fast.
- Learn about the [Bing Search APIs](../bing-web-search/bing-api-comparison.md) where you can use the suggested search strings.
- Learn about [use and display requirements](use-display-requirements.md) for Bing Search APIs.  
- Review [Autosuggest API v7 reference](reference/endpoints.md) documentation.  
