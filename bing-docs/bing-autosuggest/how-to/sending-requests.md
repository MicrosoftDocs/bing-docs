---
title: "Sending requests to the Bing Autosuggest API"
titleSuffix: Bing Search Services
description: The Bing Autosuggest API returns a list of suggested queries based on the partial query string in the search box. Learn more about sending requests.
services: cognitive-services
author: swhite-msft
manager: ehansen

ms.service: bing-search-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Sending requests to the Bing Autosuggest API.

If your application sends queries to any of the Bing Search APIs, you can use the Bing Autosuggest API to improve your users' search experience. The Bing Autosuggest API returns a list of suggested queries based on the partial query string in the search box. As characters are entered into a search box in your application, you can display suggestions in a drop-down list. Use this article to learn more about sending requests to this API. 

## Bing Autosuggest API Endpoint

The **Bing Autosuggest API**  includes one endpoint, which returns a list of suggested queries from a partial search term.

To get suggested queries using the Bing API, send a `GET` request to the following endpoint. Use the headers and URL parameters to define further specifications.

**Endpoint:** Returns search suggestions as JSON results that are relevant to the user's input defined by `?q=""`.

```http
GET https://api.bing.microsoft.com/bing/v7.0/Suggestions 
```

For details about headers, parameters, market codes, response objects, errors, etc., see the [Bing Autosuggest API v7](../reference/endpoints.md) reference.

The **Bing** APIs support search actions that return results according to their type. All search endpoints return results as JSON response objects.
All endpoints support queries that return a specific language and/or location by longitude, latitude, and search radius.

For complete information about the parameters supported by each endpoint, see the reference pages for each type.
For examples of basic requests using the Autosuggest API, see [Autosuggest Quickstarts](../quickstarts/rest/csharp.md).

## Bing Autosuggest API requests

> [!NOTE]
> * Requests to the Bing Autosuggest API must use the HTTPS protocol.

We recommend that all requests originate from a server. Distributing the key as part of a client application provides more opportunity malicious third-party access. Additionally, making calls from a server provides a single upgrade point for future updates.

The request must specify the [q](../reference/query-parameters.md#query) query parameter, which contains the user's partial search term. Although it's optional, the request should also specify the [mkt](../reference/query-parameters.md#mkt) query parameter, which identifies the market where you want the results to come from. For a list of optional query parameters, see [Query Parameters](../reference/query-parameters.md). All query parameter values must be URL encoded.

The request must specify the [Ocp-Apim-Subscription-Key](../reference/headers.md#subscriptionkey) header. Although optional, you are encouraged to also specify the following headers:

- [User-Agent](../reference/headers.md#useragent)
- [X-MSEdge-ClientID](../reference/headers.md#clientid)
- [X-Search-ClientIP](../reference/headers.md#clientip)
- [X-Search-Location](../reference/headers.md#location)

The client IP and location headers are important for returning location aware content.

For a list of all request and response headers, see [Headers](../reference/headers.md).

> [!NOTE]
> When you call the Bing Autosuggest API from JavaScript, your browser's built-in security features might prevent you from accessing the values of these headers.

To resolve this, you can make the Bing Autosuggest API request through a CORS proxy. The response from such a proxy has an `Access-Control-Expose-Headers` header that identifies response headers and makes them available to JavaScript.

It's easy to install a CORS proxy to allow our [tutorial app](../tutorial/autosuggest.md) to access the optional client headers. First, if you don't already have it, <a href="https://nodejs.org/en/download/" target="_blank">install Node.js</a>. Then enter the following command at a command prompt.

```console
npm install -g cors-proxy-server
```

Next, change the Bing Autosuggest API endpoint in the HTML file to:

```http
http://localhost:9090/https://api.bing.microsoft.com/bing/v7.0/Suggestions
```

Finally, start the CORS proxy with the following command:

```console
cors-proxy-server
```

Leave the command window open while you use the tutorial app; closing the window stops the proxy. In the expandable HTTP Headers section below the search results, you can now see the `X-MSEdge-ClientID` header (among others) and verify that it is the same for each request.

Requests should include all suggested query parameters and headers. 

The following example shows a request that returns the suggested query strings for *sail*.

> ```http
> GET https://api.bing.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE
> X-MSEdge-ClientIP: 999.999.999.999
> X-Search-Location: lat:47.60357;long:-122.3295;re:100
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
> Host: api.bing.microsoft.com
> ```

If it's your first time calling any of the Bing APIs, don't include the client ID header. Only include the client ID header if you've previously called a Bing API and Bing returned a client ID for the user and device combination.

The following web suggestion group is a response to the above request. The group contains a list of search query suggestions, with each suggestion including a `displayText`, `query`, and `url` field.

The `displayText` field contains the suggested query that you'd use to populate your search box's drop-down list. You must display all suggestions that the response includes, and in the given order.  

If the user selects a query from the drop-down list, you can use it to call the one of the [Bing Search APIs](../../bing-web-search/bing-api-comparison.md) and display the results yourself, or send the user to the Bing results page using the returned `url` field.

[!INCLUDE [bing-url-note](../../../includes/bing-url-note.md)]

```json
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "Suggestions",
    "queryContext" : {
        "originalQuery" : "sail"
    },
    "suggestionGroups" : [{
        "name" : "Web",
        "searchSuggestions" : [{
            "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
            "displayText" : "sailing lessons seattle",
            "query" : "sailing lessons seattle",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+moon+news&FORM=USBAPI",
            "displayText" : "sailor moon news",
            "query" : "sailor moon news",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+jack%27s+lincoln+city&FORM=USBAPI",
            "displayText" : "sailor jack's lincoln city",
            "query" : "sailor jack's lincoln city",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailing+anarchy&FORM=USBAPI",
            "displayText" : "sailing anarchy",
            "query" : "sailing anarchy",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale&FORM=USBAPI",
            "displayText" : "sailboats for sale",
            "query" : "sailboats for sale",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailstn.mylabsplus.com&FORM=USBAPI",
            "displayText" : "sailstn.mylabsplus.com",
            "query" : "sailstn.mylabsplus.com",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailusfood&FORM=USBAPI",
            "displayText" : "sailusfood",
            "query" : "sailusfood",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale+seattle&FORM=USBAPI",
            "displayText" : "sailboats for sale seattle",
            "query" : "sailboats for sale seattle",
            "searchKind" : "WebSearch"
        }]
    }]
}
```

## Next steps

- [What is Bing Autosuggest?](../overview.md)
- [Bing Autosuggest API v7 reference](../reference/endpoints.md)
- [Getting suggested search terms from Bing Autosuggest API](get-suggestions.md)