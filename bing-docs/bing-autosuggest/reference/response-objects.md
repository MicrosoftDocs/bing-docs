---
title: Bing Autosuggest API v7 response objects
titleSuffix: Bing Services
description: Describes the response objects that Bing Autosuggest API may return in the JSON response.
author: alekhyasasi
manager: ehansen
ms.service: bing-search-services
ms.subservice: bing-autosuggest
ms.topic: reference
ms.date: 06/12/2023
---

# Autosuggest API v7 response objects

For a list of possible objects, see **In this article** in the right pane.

If the request succeeds, the top-level object in the response is the [Suggestions](#suggestions) object. And if the request fails, the top-level object in the response is the [ErrorResponse](#errorresponse) object.

> [!NOTE]
> Because URL formats and parameters are subject to change without notice, use all URLs as-is. You should not take dependencies on the URL format or parameters except where noted.

## Error  

Defines the error that occurred.  
  
|Name|Value|Type
|-|-|-  
|<a name="error-code"></a>code|The error code that identifies the category of error. For a list of possible codes, see [Error codes](error-codes.md#error-codes).|String
|<a name="error-message"></a>message|A description of the error.|String
|<a name="error-moredetails"></a>moreDetails|A description that provides additional information about the error.|String
|<a name="error-parameter"></a>parameter|The query parameter in the request that caused the error.|String
|<a name="error-subcode"></a>subCode|The error code that identifies the error. For example, if `code` is InvalidRequest, `subCode` may be ParameterInvalid or ParameterInvalidValue.|String
|<a name="error-value"></a>value|The query parameter's value that was not valid.|String
  
## ErrorResponse  

The top-level object that the response includes when the request fails.  
  
|Name|Value|Type
|-|-|-
|_type|Type hint, which is set to ErrorResponse.|String
|<a name="errors"></a>errors|A list of errors that describe the reasons why the request failed.|[Error](#error)[]  
  
## QueryContext  

Defines the query string that Bing used for the request.
  
|Name|Value|Type
|-|-|-
|adultIntent|Not used.|Boolean
|alterationOverrideQuery|Not used.|String
|alteredQuery|Not used.|String
|askUserForLocation|Not used.|String
|originalQuery|The user's query term.|String
  
## SearchAction  

Defines a query search suggestion.  
  
|Name|Value|Type
|-|-|-
|displayText|The suggested query term to display in a user interface. |String
|<a name="searchaction-query"></a>query|The suggested query term.<br/><br/>If the user selects the query term from the list of suggestions, use the term in a Bing API request and display the search results yourself. Or, use the URL in the `url` field to send the user to the Bing search results page for the suggested query.|String
|<a name="searchaction-searchkind"></a>searchKind|The type of suggestion. The following are the possible values:<ul><li>CustomSearch &mdash; The suggestion is from a non-web search suggestion data source.</li><li>WebSearch &mdash; The suggestion is from a web search suggestion data source.</li></ul>|String
|<a name="searchaction-url"></a>url|The URL that takes the user to the Bing search results page for the suggested query.|String
  
## SuggestionGroup  

Defines a group of suggestions of the same type.  
  
|Name|Value|Type
|-|-|-
|<a name="suggestgroup-name"></a>name|The name of the group. The name identifies the type of suggestions that the group contains. For example, web search suggestions. The following are the possible group names:<ul><li>Custom &mdash; The group contains suggestions from a non-web search suggestions data source.</li><li>Web &mdash; The group contains web search suggestions.</li></ul>|String
|<a name="suggestgroup-searchsuggestions"></a>searchSuggestions|A list of up to 8 suggestions. If there are no suggestions, the array is empty.<br/><br/>You must display all suggestions in the order provided. The list is in order of decreasing relevance. The first suggestion is the most relevant and the last suggestion is the least relevant. The size of the list is subject to change.|[SearchAction](#searchaction)[]  
  
## Suggestions  

The top-level object that the response includes when the request succeeds.  
  
If the service suspects a denial of service attack, the request succeeds (HTTP status code is 200 OK). However, the body of the response is empty.  
  
|Name|Value|Type
|-|-|-
|_type|The type hint, which is set to Suggestions.|String
|queryContext|The user's query string.|[QueryContext](#querycontext)
|<a name="suggestions-suggestiongroups"></a>suggestionGroups|A list of suggested query strings grouped by type. For example, web search suggestions.|[SuggestionGroup](#suggestiongroup)[]
