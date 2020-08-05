---
title: Autosuggest search terms - Bing Web Search API
titleSuffix: Bing Search Services
description: Pair the Bing Web Search API with the Bing Autosuggest API to provide users with an enhanced search experience.
services: bing-search-services
author: swhite-msft
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/15/2020
ms.author: scottwhi
---

# Autosuggest Bing search terms in your application

If you provide a search box where the user enters their search term, use the [Bing Autosuggest API](../bing-autosuggest/overview.md) to improve the experience. The API returns suggested query strings based on partial search terms as the user types.

After the user enters a search term, it must be URL encoded before the [q](reference/query-parameters.md#query) query parameter is set. For example, if the user enters *sailing dinghies*, set `q` to `sailing+dinghies` or `sailing%20dinghies`.

If the query term contains a spelling mistake, the search response includes a [QueryContext](reference/response-objects.md#querycontext) object. The object shows the original spelling and the corrected spelling that Bing used for the search.

```json
"queryContext": {
    "originalQuery": "sialing dingy for sale",
    "alteredQuery": "sailing dinghy for sale",
    "alterationOverrideQuery": "+sialing +dingy for sale"
}
```

You can use this information to let the user know that you modified their query string when you display the search results.

![Query context UX example](media/bing-web-api/bing-query-context.PNG)  

## Next steps  

* Review sample [Bing Web Search API responses](search-responses.md).  

## See also  

* [Bing Web Search API reference](reference/endpoints.md)
